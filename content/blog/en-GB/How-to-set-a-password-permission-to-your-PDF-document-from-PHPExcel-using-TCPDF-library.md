# How to set a password / permission to your PDF document from PHPExcel (using TCPDF library) #

If you are using PHPExcel to generate PDF documents, you should know what you are actually using the TCPDF library which comes bundled in PHPExcel.

<!--BREAK-->

Although not very nice because it involves a bit of hacking the base code of PHPExcel but here is how you can do it:

For version PHPExcel v 1.7.6 the place to start hacking would be in this file:

    Classes/PHPExcel/Writer/PDF.php

at about line 272 insert the following after the TCPDF object is created:

    $pdf->SetProtection($permissions=array('print', 'copy'), $user_pass='', $owner_pass=null, $mode=0, $pubkeys=null);

More details about valid parameters can be found in the TCPDF Documentation.

The main disadvantage would be that this rules will apply to all PDF documents generated.

The best (and more complicated method) would be to call the library TCPDF directly as it is shown in this example.