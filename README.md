



# prueba2bTE

_Esta es una aplicación CRUD de calificaciones desarrollada en Angular, ionic conectada con Firebase._

## Código  🚀

_A continuación se muestra ciertas partes del código que le dan la funcionalidad mas importante._

### Vista de todos los estudiantes

_Esta parte del código se crea los metodos CRUD para crear un estudiante, obtener todos los estudiantes o uno específico, actualizar los campos y borrar
cada uno de ellos en el servicio de `appointment.service.ts`._
```
** Create **
  createBooking(apt: Appointment) {
    return this.bookingListRef.push({
      name: apt.name,
      email: apt.email,
      mobile: apt.mobile,
      professor: apt.professor
    })
  }

  ** Get Single **
   getBooking(id: string) {
    this.bookingRef = this.db.object('/appointment/' + id);
    return this.bookingRef;
  }

  ** Get List **
  getBookingList() {
    this.bookingListRef = this.db.list('/appointment');
    return this.bookingListRef;
  }

  ** Update **
  updateBooking(id, apt: Appointment) {
    return this.bookingRef.update({
      name: apt.name,
      email: apt.email,
      mobile: apt.mobile,
      professor: apt.professor

    })
  }

 ** Delete **
  deleteBooking(id: string) {
    this.bookingRef = this.db.object('/appointment/' + id);
    this.bookingRef.remove();
  }
```

### Vista de todos los estudiantes

_Esta parte del código obtiene la lista de estudiantes almacenada en Firebase mediante la llamada del método en `home.page.ts`._
```
 ngOnInit() {
    this.fetchBookings();
    let bookingRes = this.aptService.getBookingList();
    bookingRes.snapshotChanges().subscribe(res => {
      this.Bookings = [];
      res.forEach(item => {
        let a = item.payload.toJSON();
        a['$key'] = item.key;
        this.Bookings.push(a as Appointment);
      })
    })
  }
```
<p align="center"> 
 <img src="https://elvismpq.github.io/prueba2bTE/imagenes/Captura.PNG" width="300"/> 
</p> 


### Crea un estudiante nuevo

_Esta parte del código llama al metodo para crear un estudiante en `make-appointment.page.ts`._
```
formSubmit() {
    if (!this.bookingForm.valid) {
      return false;
    } else {
      this.aptService.createBooking(this.bookingForm.value).then(res => {
        console.log(res)
        this.bookingForm.reset();
        this.router.navigate(['/home']);
      })
        .catch(error => console.log(error));
    }
  }
}
```
<p align="center"> 
 <img src=" https://elvismpq.github.io/prueba2bTE/imagenes/Captura2.PNG" width="300"/> 
</p> 

### Actualizar estudiante

_En el constructor de `edit-appointment.page.ts` se recibe de parametro el id para llamar al método que retorna a un estudiante especifico, para que se aplique los cambios en `updateForm()`._
```
constructor(
    private aptService: AppointmentService,
    private actRoute: ActivatedRoute,
    private router: Router,
    public fb: FormBuilder
  ) {
    this.id = this.actRoute.snapshot.paramMap.get('id');
    this.aptService.getBooking(this.id).valueChanges().subscribe(res => {
      this.updateBookingForm.setValue(res);
    });
  }
  .
  .
  .
  .
   updateForm() {
    this.aptService.updateBooking(this.id, this.updateBookingForm.value)
      .then(() => {
        this.router.navigate(['/home']);
      })
      .catch(error => console.log(error));
  }
```
<p align="center"> 
 <img src=" https://elvismpq.github.io/prueba2bTE/imagenes/Captura3.PNG" width="300"/> 
</p> 


### Borrar estudiantes

_Esta parte del código permite borrar un estudiante mediante su id en `home.page.ts`._
```
deleteBooking(id) {
    console.log(id)
    if (window.confirm('Do you really want to delete?')) {
      this.aptService.deleteBooking(id)
    }
  }
```
<p align="center"> 
 <img src=" https://elvismpq.github.io/prueba2bTE/imagenes/Captura4.PNG" width="300"/> 
</p> 

## Autores ✒️

_Menciona a todos aquellos que ayudaron a levantar el proyecto desde sus inicios_

* **Aaron Cruz**
* **Elvis Pérez** 
* **Josselyn Vela**
* **Christian Mainato**


## Referencias de desarrollo 🛠️

* https://www.positronx.io/build-ionic-firebase-crud-app-with-angular/
