# k8sNwRtlSdr.com
RTL-SDR.COM based version of napervilleweather.com, served as rtlsdr.napervilleweather.com.
Copied from k8sNw.com (napervilleweather.com, left untouched).

- Image: jkozik/nwrtlsdr.com:v1
- Deployment/Service: nwrtlsdr (NodePort, port 80)
- HTTPRoute: nwrtlsdr-route -> rtlsdr.napervilleweather.com via weather-gateway (envoy-gateway-system)
- PV/PVC: nwrtlsdr-persistent-storage (ReadOnlyMany, mounted readOnly at /var/www/html/mount)
  NFS 192.168.100.153:/home/nfs/weather-stations/weewxrtlsdr/public_html
  (written by the ftp-weewxrtlsdr deployment)

Deploy: `kubectl apply -f .`
