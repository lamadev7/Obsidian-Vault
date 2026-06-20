---
type: code-note
domain: DocumentValidation
source: portpro-backend (load-service, ai-rule-processor) + portpro-frontend (NewDocumentsTab)
updated: 2026-06-02
---

# Runtime Rule Firing — code walk

> Explains: [[Runtime Rule Firing]]

```text
// portpro-backend/server/modules/load/load-service.js (firing chain)
PATCH tms/editTMSLoad
  load-service.js:3977  → processPreAIRules(LOAD, [UPDATE], ctx)   // filter applicable rules
                        → persist load
  load-service.js:4759  → processPostAIRules(LOAD, [UPDATE], ctx)
        → ai-rule-processor/index.js : filter module=load · customer match · status=approved · isActive
        → rule-executor.js : validation_script(ctx)=true → execution_script → publishException('DOC_REQUIREMENT')
        → TOPICS.EXCEPTION.UPDATE → billingexceptions insert → Control Tower badge
```
**The firing path.** Pre-rules filter + persist, then post-rules select approved+active rules for the customer and run the script chain. The gate is `status='approved' AND isActive` only — `engineering_approved`/`business_approved` are NOT consulted here (see [[Runtime Validation Layers and Load Match]]). Refs: `ai-rule-processor/index.js`, `rule-executor.js`, `DOC_REQUIREMENT` in `portpro-mcp/.../exception.constant.ts`.

```js
// portpro-frontend/src/pages/tms/Load/NewDocumentsTab/index.js:185-225
componentDidMount() {
  // …
  if (isDocRequiredAgentEnabled() && this.props.selectedLoads?._id) {
    getRequireDocumentsList(this.props.selectedLoads._id).then((res) => {
      const docRulesMapper = {};
      res?.data?.forEach((docRule) =>
        docRule?.doc_requirements?.forEach((req) =>
          req?.required_data?.forEach((docType) => {
            docRulesMapper[docType] = {
              isEventRule: req?.rules?.is_event_rule,
              eventIds: [...(docRulesMapper[docType]?.eventIds ?? []), ...(req?.appliedEvents ?? [])],
            };
          })));
      this.setState({ requiredDocumentsFromDocAgent: res?.data, docRulesMapper });
    });
  }
}
```
**Mount → fetch requirements.** Behind the `isDocRequiredAgentEnabled()` flag (off → legacy `billToId.requiredDocList`). Builds `docRulesMapper`: doc type → how it triggers (event rule? which events). `requiredDocumentsFromDocAgent` feeds the renderer.

```js
// portpro-frontend/src/pages/tms/Load/Billing/actionCreator.js:722-741
export const getRequireDocumentsList = (loadId) => {
  const url = `load-doc-requirements?loadId=${loadId}`;
  return new Promise((resolve, reject) => {
    HTTP('GET', url, null, { authorization: getStorage('token') })
      .then((r) => { if (r?.status === 200) resolve(r?.data); })
      .catch(reject);
  });
};
```
**Requirement fetch.** `GET /load-doc-requirements?loadId=…` — the **Node** backend (not the AI agent). Returns `load_doc_requirements` profiles: each has `required_data` (doc types), `appliedEvents` (driverOrder ids), and `rules.is_event_rule`.

```js
// portpro-frontend/.../NewDocumentsTab/AllUploadedDocumentsSandbox.js:90-180
if (isEventTypeSection) {
  reqList = docRequirements.reduce((acc, req) => {
    const applies = req.appliedEvents?.includes(chargeDetails._id);   // this DELIVERLOAD event
    return applies && req.required_data ? [...acc, ...req.required_data] : acc;
  }, []);
} else {
  reqList = docRequirements.reduce((acc, req) => {
    const applies = req.appliedChargeSets?.includes(chargeDetails._id) && !req?.rules?.is_event_rule;
    return applies && req.required_data ? [...acc, ...req.required_data] : acc;
  }, []);
}
const uploaded = _.uniq(props.listDocuments.map((d) => d?.type));
const missingRequiredDocs = reqList.filter((t) => !uploaded.includes(t));
```
**Compute what's missing.** Event path matches by `appliedEvents` (our DELIVERLOAD → POD); charge path matches by `appliedChargeSets`. Diff required-vs-uploaded → `missingRequiredDocs`. If a `DOC_REQUIREMENT` billing exception exists, that list is preferred (carries the exception id).

```jsx
// AllUploadedDocumentsSandbox.js (render — shape)
{isReqOrDocs && requireDocumentList.map((doc) => (
  <div className="required-doc-row" key={doc.name || doc}>
    <span className="badge badge-soft-danger">Required: {doc.name || doc}</span>
    <Dropzone onDrop={(files) => handleUploadDoc(files, doc, chargeDetails)}
              accept={{ 'image/*': [], 'application/pdf': [] }} />
  </div>
))}
```
**The drop-zone.** One `Dropzone` per missing required doc. `handleUploadDoc` → `tms/uploadDocumentForLoad` (`document-controller.js:2556`) → creates `DocumentValidationState{PENDING}` and kicks the AI validation chain. Verdict handling: [[Runtime Validation Layers and Load Match]].
