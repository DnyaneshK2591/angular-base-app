To add an "Apply" button to the reusable Angular multi-select dropdown component — so the selected items are only emitted when the user explicitly clicks Apply — follow this updated version:


---

✅ Updated Reusable Multi-Select Dropdown (with Apply button)

🔧 Component TS: multi-select-dropdown.component.ts

import { Component, Input, Output, EventEmitter, OnInit } from '@angular/core';

@Component({
  selector: 'app-multi-select-dropdown',
  templateUrl: './multi-select-dropdown.component.html',
  styleUrls: ['./multi-select-dropdown.component.scss']
})
export class MultiSelectDropdownComponent implements OnInit {
  @Input() options: any[] = [];
  @Input() labelKey: string = 'label';
  @Input() valueKey: string = 'value';
  @Input() placeholder: string = 'Select options';
  @Input() selectedValues: any[] = [];

  @Output() selectionChange = new EventEmitter<any[]>();

  tempSelection: any[] = [];
  isDropdownOpen = false;

  ngOnInit() {
    this.tempSelection = [...this.selectedValues];
  }

  toggleSelection(option: any) {
    const value = option[this.valueKey];
    const index = this.tempSelection.indexOf(value);
    if (index > -1) {
      this.tempSelection.splice(index, 1);
    } else {
      this.tempSelection.push(value);
    }
  }

  isSelected(option: any): boolean {
    return this.tempSelection.includes(option[this.valueKey]);
  }

  applySelection() {
    this.selectedValues = [...this.tempSelection];
    this.selectionChange.emit(this.selectedValues);
    this.isDropdownOpen = false;
  }

  toggleDropdown() {
    this.isDropdownOpen = !this.isDropdownOpen;
    if (this.isDropdownOpen) {
      this.tempSelection = [...this.selectedValues]; // reset to current state
    }
  }
}


---

🧾 Component HTML: multi-select-dropdown.component.html

<div class="dropdown-container">
  <div class="dropdown-toggle" (click)="toggleDropdown()">
    {{ selectedValues.length ? selectedValues.length + ' selected' : placeholder }}
    <span class="arrow">&#9662;</span>
  </div>

  <div class="dropdown-menu" *ngIf="isDropdownOpen">
    <div class="option" *ngFor="let option of options">
      <label>
        <input 
          type="checkbox" 
          [checked]="isSelected(option)"
          (change)="toggleSelection(option)" />
        {{ option[labelKey] }}
      </label>
    </div>
    <div class="dropdown-actions">
      <button (click)="applySelection()">Apply</button>
    </div>
  </div>
</div>


---

🎨 Component CSS: multi-select-dropdown.component.scss

.dropdown-container {
  position: relative;
  width: 200px;
}

.dropdown-toggle {
  border: 1px solid #ccc;
  padding: 8px;
  border-radius: 5px;
  background-color: white;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
}

.dropdown-menu {
  position: absolute;
  width: 100%;
  background-color: white;
  border: 1px solid #ccc;
  margin-top: 5px;
  padding: 8px;
  z-index: 1000;
  max-height: 250px;
  overflow-y: auto;
}

.option {
  padding: 4px 0;
}

.dropdown-actions {
  margin-top: 8px;
  text-align: right;
}

button {
  padding: 5px 10px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}


---

🧪 Example Usage in Parent Component

<app-multi-select-dropdown
  [options]="dropdownOptions"
  [selectedValues]="selectedItems"
  [labelKey]="'name'"
  [valueKey]="'id'"
  placeholder="Choose options"
  (selectionChange)="onSelectionChanged($event)">
</app-multi-select-dropdown>

dropdownOptions = [
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Cherry' },
  { id: 4, name: 'Date' }
];

selectedItems: number[] = [];

onSelectionChanged(selected: number[]) {
  console.log('Applied Items:', selected);
}


---

✅ Features Now Included:

Apply button for final selection.

Temporary buffer (tempSelection) so changes aren't emitted immediately.

Reusable input keys (labelKey, valueKey).

Dynamic placeholder + open/close toggle.



---

Let me know if you want:

Search bar

Select All / Deselect All

Animations or outside click to close


I can add those in a snap.

