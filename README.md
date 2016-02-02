# HttpsTestingUtil

When the server is in testing status, it need to avoid SSL checking specially using Volley.

此类作用是避開android對服務器SSL的檢查。

Usage:
用法：

    DefaultHttpClient httpClient = getNewHttpClient();
    
    HttpClientStack stack = new HttpClientStack(httpClient);
    
    RequestQueue requestQueue = Volley.newRequestQueue(context, stack);
    
    
    
    In addition:
    補充：
    1. 設置httpclient繞開https對host檢查的一般方法：
    
      DefaultHttpClient httpClient = new DefaultHttpClient();
    
      SSLSocketFactory sf = (SSLSocketFactory)
      httpClient.getConnectionManager().getSchemeRegistry().getScheme("https").getSocketFactory();
      sf.setHostnameVerifier(SSLSocketFactory.ALLOW_ALL_HOSTNAME_VERIFIER);
      
      雖然方法已經過時，但是在測試中還是可以使用的。
    
    2. 設置訪問密碼
    Credentials credentials = new UsernamePasswordCredentials(CredentialsUsername, CredentialsPassword);
    httpClient.getCredentialsProvider().setCredentials(AuthScope.ANY, credentials);
    （以上方法已經deprecated：在volley中可以使用：
    
     @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> params = new HashMap<>();
                params.put(
                        "Authorization",
                        String.format("Basic %s", Base64.encodeToString(
                                String.format("%s:%s", Setting.CredentialsUsername, Setting.CredentialsPassword).getBytes(),
                                Base64.DEFAULT)));
                return params;
            }
            
    ）
    
    
