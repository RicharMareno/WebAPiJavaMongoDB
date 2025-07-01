<h1 align="center"> <img src="https://github.com/RicharMareno/WebAPiJavaMongoDB/blob/main/LogoSiwat.svg" width=30> WebAPiJavaMongoDB <img src="https://github.com/RicharMareno/WebAPiJavaMongoDB/blob/main/LogoSiwat.svg" width=30></h1>
 
## Manejo de datos con JavaSprig , MongoDB , ApacheToncat 
### [JavaSprig](https://spring.io/tools) 
### [MongoDB](https://www.mongodb.com/docs/manual/installation/)
##
## Servicios de salida para realizar pruebas
### [Lista de usuarios](http://www.siwat.es:8080/api/Usuarios)  
## Usuarios: UsuariosService
``` java
@Service
@RequiredArgsConstructor  //crea un constructor para que la variable se inicialice con ese dato la proxima vez
public class UsuariosService {	
	private final UsuariosRepository usuariosRepository;	
    @Autowired
    public UsuariosService(UsuariosRepository usuariosRepository) {
        this.usuariosRepository = usuariosRepository;
    }	
	public void save(Usuarios usuario) {
		usuariosRepository.save(usuario);
	}	
	public List<Usuarios> findAll(){
		return usuariosRepository.findAll();
	}
	public Optional<Usuarios> findById(String id){
		return usuariosRepository.findById(id);	
	}	
	public Usuarios getUsuarioById(String id) 
	{ return usuariosRepository.findById(id).orElse(null); }				
	// eliminar una dato por el id
	public void deleteById(String id){
		usuariosRepository.deleteById(id); 		
	}
}
```
### [Lista de imagenes guardadas](http://www.siwat.es:8080/api/JSonImgWeb)
## JSonImgWeb: JSonImgWebService
``` java
@RestController
@RequestMapping("/api/JSonImgWeb")
public class JSonImgWebService {
	private final JSonImgWebRepository jSonImgWebRepository;
	
    @Autowired
    public JSonImgWebService(JSonImgWebRepository jSonImgWebRepository) {
        this.jSonImgWebRepository = jSonImgWebRepository;
    }
	
	public void save(JSonImgWeb jSonImgWeb) {
		jSonImgWebRepository.save(jSonImgWeb);
	}
	
	public List<JSonImgWeb> findAll(){
		return jSonImgWebRepository.findAll();
	}

	public Optional<JSonImgWeb> findById(String id){
		return jSonImgWebRepository.findById(id);	
	}
	
	public JSonImgWeb getJSonImgWebById(String id) 
	{ return jSonImgWebRepository.findById(id).orElse(null); }
				
	public void deleteById(String id){
		jSonImgWebRepository.deleteById(id); 		
	}
}
```

# Diccionario de las tablas

 ## Tabla Usuarios
 |    campo   |   tipo   |          descripcion          |
 |------------|----------|-------------------------------|
 |+ usuarioId |  string  | Indentificacdor del ususario. |
 |+ nombre    |  string  | Nombre de la usuario.         |
 |+ email     |  string  | Correo electronico.           |

## JSon: Como llenaria
``` json
{
  "usuarioId": "1",
  "nombre": "ABCD pruebas",
  "email": "PruebasABC@gmail.com"
}
```
## Como quedaria en el MongoDB
``` json
{
  "_id": {
    "$oid": "673f91f57c06df78a784b25c"
  },
  "usuarioId": "1",
  "nombre": "ABCD pruebas",
  "email": "PruebasABC@gmail.com",
  "_class": "com.prueba1.model.Usuarios"
}
```
 ## Tabla JSonImgWeb
 |    campo   |   tipo   |                    descripcion                 |
 |------------|----------|------------------------------------------------|
 |+ codigo    |  string  | Código de las imagenes.                        |
 |+ nombre    |  string  | Nombre de la provincia.                        |
 |+ tipo      |  string  | Es el tipo de imagen del archivo que se genero.|
 |+ archivo   |  string  | Es una imagen codificada en base64.            |
 |+ fch_cre   |  string  | Fecha de creacion del Archivo.                 |
 |+ fch_act   |  string  | Fecha de Actualizacion del Archivo.            |

##  Como llenaria
``` json
{
  "codigo": "ImgPrb",
  "nombre": "JSonRems",
  "tipo": "jpg",
  "archivo": "data:image/jpg;base64,/9j/4........RgABAQE",
 "fch_cre": "2023-11-22 00:29:41",
  "fch_act": "2023-11-22 00:29:41",
  "_class": "com.prueba1.model.JSonImgWeb"
}
```
##  Como quedaria en el MongoDB
``` json
{
  "_id": {
    "$oid": "673f9ac96889de4ba87b6d45"
  },
  "codigo": "ImgPrb",
  "nombre": "JSonRems",
  "tipo": "jpg",
  "archivo": "data:image/jpg;base64,/9j/4........RgABAQE",
 "fch_cre": "2023-11-22 00:29:41",
  "fch_act": "2023-11-22 00:29:41",
  "_class": "com.prueba1.model.JSonImgWeb"
}
```
<h1 align="center"> <img src="https://github.com/RicharMareno/WebAPiJavaMongoDB/blob/main/LogoSiwat.svg" width=20> Consumir los datos </h1>
<h2 align="center"> Consumir desde un Curl </h2>
<h2 align="left"> Metodo Post </h2>

``` php
curl --location 'http:www.siwat.es:8080/api/{Usuarios/JSonImgWeb}' \
--header 'Content-Type: application/json' \
--data-raw 'JSon'
```
<h2 align="left"> Metodo Get </h2>

``` php
curl --location 'http:www.siwat.es:8080/api/{Usuarios/JSonImgWeb}'
```
<h2 align="center"> Consumir desde un C# HttpClient </h2>
<h2 align="left"> Metodo Post </h2>

``` c#
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Post, "http:www.siwat.es:8080/api/{Usuarios/{Usuarios/JSonImgWeb}}");
var content = new StringContent( "String JSon", null, "application/json");
request.Content = content;
var response = await client.SendAsync(request);
response.EnsureSuccessStatusCode();
Console.WriteLine(await response.Content.ReadAsStringAsync());
```
<h2 align="left"> Metodo Get </h2>

``` c#
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "http:www.siwat.es:8080/api/{Usuarios/JSonImgWeb}");
var response = await client.SendAsync(request);
response.EnsureSuccessStatusCode();
Console.WriteLine(await response.Content.ReadAsStringAsync());
```
<h2 align="center"> Consumir desde Python - Requests </h2>
<h2 align="left"> Metodo Post </h2>

``` Python
import requests
import json
url = "http:www.siwat.es:8080/api/{Usuarios/JSonImgWeb}}"
payload = json.dumps(JSon)
headers = {
  'Content-Type': 'application/json'
}
response = requests.request("POST", url, headers=headers, data=payload)
print(response.text)
```
<h2 align="left"> Metodo Get </h2>

``` Python
import requests
url = "http:www.siwat.es:8080/api/{Usuarios/JSonImgWeb}"
payload = {}
headers = {}
response = requests.request("GET", url, headers=headers, data=payload)
print(response.text)
```
<h1 align="center"> <img src="https://github.com/RicharMareno/WebAPiJavaMongoDB/blob/main/LogoSiwat.svg" width=20> Contacto <img src="https://github.com/RicharMareno/WebAPiJavaMongoDB/blob/main/LogoSiwat.svg" width=20></h1>

- [www.siwat.es](http:www.siwat.es):
  - `#Sugerencias` Enviarme un correo desde la pagina con sugerencias
  - `#Contribuir` Se desean contribuir avisame, te dare permisos .
- [@siwat](http:www.siwat.es/MiWeb/docusaurus) X
