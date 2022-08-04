```javascript
var entityFunctions = {
	"soapenv:Envelope":{
		"_attributes":{
			"xmlns:soapenv":"http://schemas.xmlsoap.org/soap/envelope/"
		},
		"soapenv:Header":{},	
		"soapenv:Body":{
			"core:entityFunctions":{
				"data_content":"",
				"data_type":""
			}
		}
	}
};

entityFunctions["soapenv:Envelope"]["soapenv:Body"]["core:entityFunctions"]["action"] = 1;
entityFunctions["soapenv:Envelope"]["soapenv:Body"]["core:entityFunctions"]["client_code"] = 2;
entityFunctions["soapenv:Envelope"]["soapenv:Body"]["core:entityFunctions"]["data_content"] = 3;
entityFunctions["soapenv:Envelope"]["soapenv:Body"]["core:entityFunctions"]["batchId"] = 4;
entityFunctions["soapenv:Envelope"]["soapenv:Body"]["core:entityFunctions"]["data_type"] = 5;

var xml_data = OBJtoXML (entityFunctions);

function OBJtoXML(obj) {
  var xml = '';
  for (var prop in obj) {
    xml += obj[prop] instanceof Array ? '' : "<" + prop + ">";
    if (obj[prop] instanceof Array) {
      for (var array in obj[prop]) {
        xml += "<" + prop + ">";
        xml += OBJtoXML(new Object(obj[prop][array]));
        xml += "</" + prop + ">";
      }
    } else if (typeof obj[prop] == "object") {
      xml += OBJtoXML(new Object(obj[prop]));
    } else {
      xml += obj[prop];
    }
    xml += obj[prop] instanceof Array ? '' : "</" + prop + ">";
  }
  var xml = xml.replace(/<\/?[0-9]{1,}>/g, '');
  return xml
}

$.ajax({
    url: 'corews?wsdl',
    type: 'post',
	headers: {
        'login': '***',
                                            'pass': '***'
    },
	data: xml_data,
    async: false,
    contentType: 'application/xml;charset=UTF-8',
    success: function (result) {},
    error: function (error) {}
});
```