Removing unnecessary components from the file allows us to focus on reviewing the conversations. Upon examining a TCP conversation using command `tshark`, we encounter an useful string.

~~~shell
$ tshark -r shark1.pcap -z "follow,tcp,ascii,5"

	330
HTTP/1.1 200 OK
Date: Mon, 10 Aug 2020 01:51:45 GMT
Server: Apache/2.4.29 (Ubuntu)
Last-Modified: Fri, 07 Aug 2020 00:45:02 GMT
ETag: "2f-5ac3eea4fcf01"
Accept-Ranges: bytes
Content-Length: 47
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html

Gur synt vf cvpbPGS{c33xno00_1_f33_h_qrnqorrs}
~~~

This looks like a simple substitution cipher. We can use online tools to get the key, it is `nopqrstuvwxyzabcdefghijklm`. Hence we get the flag easily.

Flag: 
`picoCTF{p33kab00_1_s33_u_deadbeef}`