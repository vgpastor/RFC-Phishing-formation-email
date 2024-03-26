# Propuesta para la Introducción de Cabeceras de Correo Electrónico para la Formación en Detección de Phishing

**Autor(es):** Víctor García Pastor

**Correo de Contacto:** vgpastor08@gmail.com 

**Área:** Aplicaciones y Servicios de Internet  

**Fecha:** 25/03/2024

## Resumen
Este documento propone la adición de nuevas cabeceras de correo electrónico diseñadas específicamente para identificar correos electrónicos que son enviados como parte de programas de formación en detección de phishing. Estas cabeceras permitirían a los destinatarios verificar la autenticidad de los correos electrónicos de formación mediante consultas DNS para confirmar que los dominios remitentes están autorizados para enviar este tipo de correos.

## Introducción
El phishing sigue siendo una de las amenazas de seguridad más significativas en Internet hoy en día. La formación de usuarios en la detección de intentos de phishing es una herramienta crucial en la defensa contra este tipo de ataques. Sin embargo, la efectividad de estos programas de formación puede verse comprometida si los correos electrónicos utilizados para la formación son incorrectamente marcados y filtrados por soluciones anti-phishing.

Este documento propone una solución para este problema mediante la introducción de cabeceras específicas en los correos electrónicos de formación que permitirían a los sistemas de correo identificarlos correctamente y evitar su filtrado, al mismo tiempo que proporciona un mecanismo de verificación para confirmar la legitimidad de estos correos.

## Objetivos
El objetivo principal de esta propuesta es mejorar la efectividad de los programas de formación en detección de phishing mediante la introducción de un mecanismo que permita a los sistemas de correo identificar y autenticar los correos electrónicos de formación, manteniendo la seguridad en los sistemas de correo y evitando el abuso de este mecanismo.

La motivación principal detrás de esta propuesta es mejorar la efectividad de los programas de formación en detección de phishing. Actualmente, los correos electrónicos utilizados en estos programas pueden ser marcados incorrectamente como intentos de phishing por soluciones anti-phishing, lo que puede llevar a que los usuarios no reciban la formación adecuada o que los correos sean filtrados antes de que puedan ser utilizados para la formación.

Por último es muy importante que los usuarios puedan utilizar los sistemas de reporte de detección de phising para mejorar el uso de estos sistemas, así como entrenar a los propios usuarios en la detección y prevención de este tipo de ataques. Estos reportes deben poder descartar los correos de formación en detección de phishing para no generar falsos positivos, así como poder reenviar dichos correos a los proveedores de formación en detección de phishing para que puedan ser analizados y mejorados.

## Descripción de la Propuesta
La propuesta consta de los siguientes elementos principales, adición de cabeceras de correo electrónico, legitimación del remitente y mecanismo de validación de enlaces.

### Adición de Cabeceras de Correo Electrónico
La propuesta incluye la introducción de dos nuevas cabeceras de correo electrónico obligatorias para los correos electrónicos de formación en detección de phishing:
- Phishing-Simulation: Obligatorio | Un identificador único para la sesión de formación. Puede ser compartido entre varios destinatarios, pero no puede ser repetido para un mismo destinatario.
- Phishing-Simulation-Provider: Obligatorio | El dominio del proveedor de formación en detección de phishing.
- Phishing-Simulation-Auth: Obligatorio | Registro DNS que contiene la información de autenticación del correo electrónico.
- Phishing-Simulation-Report: Opcional | Un enlace o email para reportar correos de formación en detección de phishing.

#### Cabecera Phishing-Simulation
Identificador único para la sesión de formación.
Este identificador permite la identificación del email como una simulación de phishing y permite a los sistemas de correo electrónico realizar consultas DNS para verificar la autenticidad del correo electrónico.
A nivel formativo se puede utilizar para medir la efectividad de la formación en detección de phishing específica.

```Phishing-Simulation-Provider: unic-identifier```

#### Cabecera Phishing-Simulation-Provider
Identificación del proveedor de formación en detección de phishing.
Este campo permite a los sistemas de correo electrónico identificar el dominio del proveedor de formación y utilizarlo para verificar la autenticidad del correo electrónico.
Una misma compañía puede tener varios proveedores de formación en detección de phishing, por lo que es importante que esta cabecera sea obligatoria.

Al utilizar los sistemas de reporte de detección de phishing por parte de los usuarios, se puede identificar a los proveedores de formación en detección de phishing que están siendo más efectivos y aquellos que necesitan mejorar. Por último se puede utilizar estas reglas para filtrar mensajes reportados.

```Phishing-Simulation-Provider: domain.com```

#### Cabecera Phishing-Simulation-Auth
Registro DNS que contiene la información de autenticación del correo electrónico.
Este registro debe ser dinámico para cada dominio puesto que permite ocultar de manera pública si un dominio utiliza este tipo de formación en detección de phishing.

``` Phishing-Simulation-Auth: xxxxxxxxxxx```

### Mecanismo de Verificación DNS
Detalla cómo los sistemas de correo pueden utilizar las cabeceras propuestas para realizar consultas DNS y verificar la autenticidad de los correos electrónicos de formación.

EL propietario del dominio debe agregar un registro TXT en su DNS con el siguiente formato:
El registro dns debe ser dinámico para cada dominio, de esta manera se puede ocultar si un dominio utiliza este tipo de formación en detección de phishing.

El contenido del registro almacena tanto los emails autorizados para el envío de los emails, como los dominios autorizados para los enlaces.

```xxxxxxxxxxx.domain.com TXT v:1; sender:domain-that-send-email.com,domain-that-send-email2.com ;links:domain-links.com,domain-links.com ```

Se hace necesario también la necesidad de que los valores puedan ser autorizados externamente, en caso de que un proveedor necesite modificar, agregar o eliminar dominios tanto para el envío como para los enlaces los clientes no tengan que modificar continuamente sus propios registros. 

Entrada en el servicio
```_pdt.domain.com TXT v:pdt1; sender:domain-that-send-email.com,domain-that-send-email2.com ;links:domain-links.com,domain-links2.com ```

Entrada en el cliente
```xxxxxxxxxxx.domain.com TXT v:pdt1; sender:domain-that-send-email3.com,_pdt.domain.com ;links:domain-links3.com,_pdt.domain.com ```


### Legitimación del Remitente
Proporciona recomendaciones sobre cómo los remitentes de correos electrónicos de formación pueden legitimar sus dominios para evitar el abuso de este mecanismo.

### Mecanismo de Validación de Enlaces
La validación de los enlaces dentro del correo electrónico es un paso importante para evitar que los usuarios hagan clic en enlaces maliciosos. Se propone la adición de un mecanismo de validación de enlaces que permita a los sistemas de correo verificar la autenticidad de los enlaces en los correos electrónicos de formación.

## Seguridad
Se han considerado las implicaciones de seguridad de esta propuesta y se han proporcionado recomendaciones para mitigar posibles riesgos.
Actualmente no se han identificado riesgos de seguridad significativos asociados con esta propuesta. Sin embargo, es importante que los remitentes de correos electrónicos de formación sigan las recomendaciones de seguridad proporcionadas en este documento para evitar el abuso de este mecanismo.

## Implementación
1. Los proveedores de formación en detección de phishing deben implementar las cabeceras de correo electrónico propuestas en sus correos electrónicos de formación.
2. Los proveedores de formación en detección de phishing deben legitimar sus dominios autorizados tanto para el envío de emails como de los enlaces para evitar el abuso de este mecanismo.
3. Los clientes deben configurar sus registros DNS con los valores necesarios para permitir la verificación de los correos electrónicos de formación.
4. El sistema de correo electrónico al recibir un correo de formación en detección de phishing debe realizar una consulta DNS para verificar la autenticidad del correo y de los enlaces contenidos en el email.
   1. Si la consulta DNS es exitosa, el correo electrónico continúa la verificación.
   2. Se comprueba si el dominio del remitente está autorizado para enviar correos de formación en detección de phishing.
   3. Se comprueba si los enlaces contenidos en el correo electrónico están autorizados para ser utilizados en correos de formación en detección de phishing.
   4. Si la verificación es exitosa, el correo electrónico se entrega al destinatario. En caso de que alguno de los anteriores pasos falle el correo debe ser marcado como sospechoso.
5. Los usuarios deben utilizar los sistemas de reporte de detección de phishing para reportar correos electrónicos sospechosos y mejorar la efectividad de los programas de formación en detección de phishing.
   1. El sistema de correo electrónico enviará el correo reportado al proveedor de formación en detección de phishing para su análisis y mejora.
   2. Si no existiera entrada para el reporte se dentendrá el envío del correo a sistemas antiphising y se marcará como sospechoso.

## Ejemplos

- provider.com -> Dominio del proveedor de formación en detección de phishing
- test.com -> Dominio utilizado para enviar el correo de formación en detección de phishing
- example.com -> Dominio utilizado para los enlaces en el correo de formación en detección de phishing

### Entradas DNS
```_pdt.provider.com TXT v:pdt1; sender:test.com ;links:example.com ```

```2654896524568._pdt.client.com TXT v:pdt1; sender:_pdt.provider.com ;links:_pdt.provider.com ```

### Cabeceras de Correo Electrónico
```
From: fake@mail.test.com
To: user@client.com
Subject: Message from CEO
Message-ID: <05c18622-f2ad-cb77-2ce9-a0bbfc7d7ad0@clear-code.com>
Date: Mon, 25 Mar 2024 10:00:00 -0400
Phishing-Simulation-Provider: provider.com
Phishing-Simulation-Auth: 2654896524568
Phishing-Simulation-Report: reports@provider.com
Content-Type: text/plain; charset=utf-8

Please send 1M USD to the following account: https://transfers.example.com/bank-account

Regards
```

## Agradecimientos
Zepo.app por la inspiración en la creación de este documento.

## Referencias
- [RFC 5322](https://tools.ietf.org/html/rfc5322) - Internet Message Format
- [RFC 7489](https://datatracker.ietf.org/doc/html/rfc7489) - Domain-based Message Authentication, Reporting, and Conformance (DMARC)
- [RFC 1035](https://tools.ietf.org/html/rfc1035) - Domain Names - Implementation and Specification
