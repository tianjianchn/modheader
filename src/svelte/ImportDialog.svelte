<script>
  import lodashIsEmpty from "lodash/isEmpty";
  import Dialog, { Title, Content, Actions } from "@smui/dialog";
  import Button, { Label } from "@smui/button";
  import IconButton from "@smui/icon-button";
  import { mdiClose, mdiCheck } from "@mdi/js";
  import MdiIcon from "./MdiIcon.svelte";
  import { DISABLED_COLOR, PRIMARY_COLOR } from "../js/constants";
  import { showMessage } from "../js/toast";
  import { overrideProfile, importProfiles } from "../js/datasource";

  let importTextbox;
  let importText;
  let dialog;

  export function show() {
    dialog.open();
  }

  function done() {
    const importedProfiles = JSON.parse(importText);
    importProfiles(importedProfiles);
    dialog.close();
    showMessage(
      `${importedProfiles.length} ${
        importedProfiles.length === 1 ? "profile" : "profiles"
      } successfully imported!`
    );
  }
</script>

<Dialog
  bind:this={dialog}
  class="import-dialog"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-content">
  <Title id="dialog-title">
    Import profile
    <IconButton
      aria-label="Close"
      class="dialog-close-button"
      on:click={() => dialog.close()}>
      <MdiIcon size="32" icon={mdiClose} color="#888" />
    </IconButton>
  </Title>
  <Content id="dialog-content">
    <textarea
      bind:this={importTextbox}
      class="extra-large-textarea"
      rows="40"
      bind:value={importText} />
  </Content>
  <div class="mdc-dialog__actions">
    <Button disabled={lodashIsEmpty(importText)} on:click={() => done()}>
      <MdiIcon
        size="24"
        icon={mdiCheck}
        color={lodashIsEmpty(importText) ? DISABLED_COLOR : PRIMARY_COLOR} />
      &nbsp;
      <Label>Import</Label>
    </Button>
  </div>
</Dialog>