# What it is

<div style="text-align: justify">

OTP signature is a solution to perform remote electronic signatures based on a Uanataca seal. These signatures come alongside with an evidences report available for each process.
<br/>
Cryptographic operations are done in Uanataca Trusted Service Center side.
</div>

# How it works

<div style="text-align: justify">
An HTTP RESTful API is exposed directly from Uanataca Trusted Service Center by means of which, business applications are enabled to perform electronic advanced signatures making use of this tool.

<br/>

For the execution of signature requests, all the data has to be sent along the credentials of the Billing account which will be used to authenticate the user. All the operations are performed in our side and the result will be a document signed by Uanataca which is a Qualified Trusted Service Provider.

<br/>
OTP Signature generates an audit trail evidence report which reflects every step performed with his belonging information. This audit trail will be obtanaible retrieving the document through an API call or scanning a QR code, this code is directly linked to the evidences and it can be found into the signed document.
</div>



# Test environment


For testing purposes, Uanataca provides integrators of a pre-configured test-mode accessible at the following URL:

</br>

	https://otponeshot.demo.bit4id.org

When using our test-mode, you must consider that Billing credentials are required.

# Webhook configuration
OTP API has the option of using a Webhook implemented on customer business side to manage its service callbacks. Every request status change will trigger a simple event-notification via HTTP POST, consisting on a JSON object to an URL that must be explicitly included as an **optional parameter** in the <a href='#tag/OTP-API/paths/~1api~1v1~1sign/post'>Sign</a> call.

The following is a sample view of the JSON object that is sent as a callback at every status change:

    {
	    "status": "incomplete",
	    "upload_date": "28/06/2022 12:56:20 UTC",
	    "returnmethod": "api",
	    "signaturetext": "CONTRACT SIGNATURE",
	    "graphometric": false,
	    "callbackaction": "view_document_by_signer_0",
	    "signers": {
		    "1": {
			    "name": "Second Signer",
			    "mobile": "+34666778406",
			    "horizontal_position": true,
		    	"position": "50, 100, 350,165",
	    		"page": 0,
    			"complete": false
		    },
		    "0": {      
			    "name": "First Signer",
			    "view_from_mobile": false,
			    "mobile": "+34666778406",
			    "view_ipaddress": "34.241.214.147",
			    "view_date": "28/06/2022 12:56:45 UTC",
			    "view_useragent": "Firefox 101.0 / Windows 10",
			    "horizontal_position": false,
			    "position": "45, 100, 100,350",
			    "page": 0,
			    "complete": false
		    }
	    },
	    "job": "5e2bec1f-01f5-4fec-9ba6-0e68a4154b7c",
	    "sendersms": "ProvSMS",
	    "mime": "application/pdf",
	    "partner": "Partner SPA",
	    "enablehardware": false,
	    "size": 129433,
	    "enableotp": true,
	    "filename": "salida (2).pdf",
	    "ext": ".pdf",
	    "current_signer": 0,
	    "document": "1daeca0437824b09867881c70097e4ac"
    }

Where:
`callbackaction` is the step of the request which triggered the information shown.</br>
`signers` will contain json objects named by numbers as signers in the request</br>
`status` is the overall status of the request</br>
`job` is the request unique id.</br>

</br>

> **Sample code**

In this sample, every JSON object is stored in a file named 'otp'.

The webhook parameter used in the <a href='#tag/OTP-API/paths/~1api~1v1~1sign/post'>Sign</a> call is defined as:

	{host}/otp

where {host} is the IP or domain from the server exposing the webhook.

</br>

*Python*

	import web
	import datetime
	
	urls = (
	        '/otp, 'otp',
	        )
	
	app = web.application(urls, globals())
	app = app.wsgifunc()
	
	class video:
		def POST(self):
			data = web.data()
			f = open("status.json",'a+')
			f.write(data)
			f.close()
			return ''

	if __name__ == "__main__":
	    app.run()


*PHP*

	<?php
	
	//otp.json

	$post = file_get_contents('php://input',true);
	$file_handle = fopen('/otp/status.json', 'w');
	fwrite($file_handle, $post);
	fclose($file_handle);
	
	?>


# Postman Collection

A postman collection is available as a support for a quick start.<br>
<a href="https://cdn.bit4id.com/es/uanataca/public/otp/Uanataca_OTP_Postman.zip">
    <img src="https://raw.githubusercontent.com/UANATACA/OTP-REPO/main/img/postman.svg" alt="postman_logo" width="260" height="130" style="display:block;">
</a>
<a href="https://cdn.bit4id.com/es/uanataca/public/otp/Uanataca_OTP_Postman.zip">OTP Postman collection download</a>

<div id="APIReference" style="padding-top: 60px;"><h1>API Reference<h1></div>
