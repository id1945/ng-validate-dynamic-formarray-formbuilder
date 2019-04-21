[Demo](https://id1945.github.io/ng-validate-dynamic-formarray-formgroup/)

##### …create new project
```
ng new ng-validate-dynamic-formarray-formgroup
cd ng-validate-dynamic-formarray-formgroup
npm install
npm start
```
##### …use code available
```
git clone https://github.com/id1945/ng-validate-dynamic-formarray-formgroup.git
cd ng-validate-dynamic-formarray-formgroup
npm install
npm start
```
##### http://localhost:4200

##### …working with github
```
git remote add origin https://github.com/id1945/ng-validate-dynamic-formarray-formgroup.git
git remote set-url origin https://github.com/id1945/ng-validate-dynamic-formarray-formgroup.git
git commit -m "first commit"
git push -u origin master
```

##### Summary

```javascript
export class AppComponent implements OnInit {
  formGroup: FormGroup;

  ngOnInit() {
    this.validate();
  }

  /********BUILD FORM-GROUP START*******/
  public validate(): void {
    this.formGroup = new FormGroup({
      formArray1: new FormArray([
        this.initX()
      ])
    });
    // this.formGroup.valueChanges.subscribe(data => console.log(data));
  }

  get f() { return this.formGroup.controls; }

  public initX(): FormGroup {
    return new FormGroup({
      X: new FormControl('X', [Validators.required, Validators.pattern('[0-9]{3}')]),
      formArray2: new FormArray([
        this.initY()
      ])
    });
  }

  public initY(): FormGroup {
    return new FormGroup({
      Y1: new FormControl('Y1', [Validators.required, Validators.pattern('[0-9]{3}')]),
      Y2: new FormControl('Y2', [Validators.required, Validators.pattern('[0-9]{3}')]),
      formArray3: new FormArray([
        this.initZ()
      ])
    });
  }

  public initZ(): FormGroup {
    return new FormGroup({
      Z: new FormControl('Z', [Validators.required, Validators.pattern('[0-9]{3}')]),
    });
  }
  /********BUILD FORM-GROUP END*******/

  /********EVENT CLICKZ START*******/
  public addX(): void {
    const control = <FormArray>this.f.formArray1;
    control.push(this.initX());
  }

  public addY(ix): void {
    const control = (<FormArray>this.f.formArray1).at(ix).get('formArray2') as FormArray;
    control.push(this.initY());
  }

  public addZ(ix, iy): void {
    const control = ((<FormArray>this.f.formArray1).at(ix).get('formArray2') as FormArray).at(iy).get('formArray3') as FormArray;
    control.push(this.initZ());
  }

  public onSubmit(e): void {
    if (this.formGroup.valid) {
      alert('Result: success!');
      console.log(e.value);
    }
  }
  /********EVENT CLICKZ END*******/

  constructor() { }

}
```

```html
<form [formGroup]="formGroup" (submit)="onSubmit(formGroup)" novalidate>
  <!-- X -->
  <div formArrayName="formArray1">
    <div *ngFor="let X of f.formArray1['controls']; let ix=index">
      <div formGroupName="{{ix}}" class="formArray1">
        <input type="text" formControlName="X">
        <p *ngIf="X['controls'].X?.errors?.required">X is required</p>
        <p *ngIf="X['controls'].X?.errors?.pattern">Error pattern</p>
        <!-- Y -->
        <div formArrayName="formArray2">
          <div *ngFor="let Y of X['controls'].formArray2['controls']; let iy=index">
            <div formGroupName="{{iy}}" class="formArray2">
              <input type="text" formControlName="Y1">
              <p *ngIf="Y['controls'].Y1?.errors?.required">Y1 is required</p>
              <p *ngIf="Y['controls'].Y1?.errors?.pattern">Error pattern</p>
              <br/>
              <input type="text" formControlName="Y2">
              <p *ngIf="Y['controls'].Y2?.errors?.required">Y2 is required</p>
              <p *ngIf="Y['controls'].Y2?.errors?.pattern">Error pattern</p>
              <!-- Z -->
              <div formArrayName="formArray3">
                <div *ngFor="let Z of Y['controls'].formArray3['controls']; let iz=index">
                  <div formGroupName="{{iz}}" class="formArray3">
                    <input type="text" formControlName="Z">
                    <p *ngIf="Z['controls'].Z?.errors?.required">Z is required</p>
                    <p *ngIf="Z['controls'].Z?.errors?.pattern">Error pattern</p>
                  </div>
                </div>
                <input type="button" (click)="addZ(ix,iy)" value="Add Z">
              </div>
              <!-- Z End -->

            </div>
          </div>
          <input type="button" (click)="addY(ix)" value="Add Y">
        </div>
        <!-- Y End-->
      </div>
    </div>
    <input type="button" (click)="addX()" value="Add X">
  </div>
  <!-- X End -->
  <button type="submit">SUBMIT</button>
<form>
```

```javascript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule, FormsModule, ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

#### https://giai-ma.blogspot.com/2018/11/deploying-angular-app-to-github-pages.html