# Fortify Issues Review Report
----

# 
This report include 3 sections

  - 3 critical reported issues.
  - Issuses by categories
  - conclusion
# 

## 3 critical reported issues
# 
# 
##### Critical Issue 1:      
# 
# 
> AFURLRequestSerialization.m, line 378 (Insecure Storage: Lacking Data Protection) On line 378 of AFURLRequestSerialization.m, the method ^{}() writes data to a file lacking sufficient encryption settings.


```sh

            NSInteger bytesWritten = [outputStream write:buffer
maxLength:(NSUInteger)bytesRead];
            if (outputStream.streamError || bytesWritten < 0) {
                error = outputStream.streamError;
```

###### It is not an issue, need not to be fixed,Resons as below: 
- *The buffer is stored in memory/cache but not file,it need not encrypt. *
- *The image/documents downloaded from sharepoint need not to be encrypted.*
 

# 
##### Critical Issue 2:      
# 
# 
> ￼AFURLConnectionOperation.m, line 664 (Insecure Storage: Lacking Data Protection).
On line 664 of AFURLConnectionOperation.m, the method connection:didReceiveData:() writes data to a file lacking sufficient encryption
settings.

```sh

664 NSInteger numberOfBytesWritten = 0;
665 while (totalNumberOfBytesWritten < (NSInteger)length) {
666 numberOfBytesWritten = [self.outputStream write:&dataBuffer[(NSUInteger)totalNumberOfBytesWritten] maxLength:(length -
                      (NSUInteger)totalNumberOfBytesWritten)];
667 if (numberOfBytesWritten == -1) {
668 break;
```

###### It is not an issue, need not to be fixed,Resons as below: 
- *The buffer is stored in memory/cache but not file,it need not encrypt. *
- *The image/documents downloaded from sharepoint need not to be encrypted.*

# 
##### Critical Issue 3:      
# 
# 
> MWPhotoBrowser.m, line 1602 (Insecure Storage: Missing Encryption on Stored Private Media)

```sh

1602 - (void)actuallySavePhoto:(id<MWPhoto>)photo {
1603 if ([photo underlyingImage]) {
1604 UIImageWriteToSavedPhotosAlbum([photo underlyingImage], self,
1605
@selector(image:didFinishSavingWithError:contextInfo:), nil);
1606 }
```

###### It is not an issue, need not to be fixed,Reson  as below: 
- *Image saved to album need not to be encrypt. *
- *It is standard usage of Apple's UIImageWriteToSavedPhotosAlbum API .*


# 

## Issuses by categories
# 
# 
There are categories as below

  - ￼Category:Null Dereference (61 Issues).
  - ￼Category: Privacy Violation: Screen Caching (42 Issues)
  - ￼Category: Type Mismatch: Signed to Unsigned (18 Issues)
  - ￼Category: Unreleased Resource: Streams (10 Issues)
  - ￼Category: Insecure Transport: HTTP GET (7 Issues)
  - ￼Category: Key Management: Hardcoded Encryption Key (4 Issues)
  - Category: Insecure Storage: Lacking Data Protection (2 Issues)
  - ￼Category: Log Forging (2 Issues)
  - ￼Category: Insecure Storage: Missing Encryption on Stored Private Media (1 Issues)
 
# 
# 
# 
# 
#####  **Category: Null Dereference (61 Issues)**
# 

*We've reviewed all the issues listed in this category, and found there none of the issue listed need to be fixed. Fortiy is too strict on NULL pointer check,but Objective-C allows an object be used before nil check. For example:*

```sh
row.item
```
*row.item is correct in Objectivce-c enven if row is nil. If row is nil,the code will do nothing. But Fortify tool will throw a issue since it thinks that row should not be nil before use. Not issues, and need not to be fixed.*

# 
# 
# 
# 
#####  ** Privacy Violation: Screen Caching (42 Issues)**
# 

*Fortify suggests that every page/view controller should add image cache protection code in projects. Actually,this is not reasonable since the background data protection should be done in system level but not app level. Apple did not mention this rule and there is no other app did like this. So we could ignore this section. Not issues, and need not to be fixed.
￼*

# 
# 
# 
#####  **  Type Mismatch: Signed to Unsigned (18 Issues)**
# 

*
Signed to Unsigned implicit type conversion is allowed in objective-C. There will be no problem if these issues remains. It is all right but not must to fix. 
￼*

# 
# 
# 
#####  **   Unreleased Resource: Streams (10 Issues)**
# 

*
Since Analytics projected is ARC enabled, the resource and memories is managed by Xcode and IOS system automatically,So they are not issues,need not to be fixed. 
￼*
# 
# 
# 
#####  **    Insecure Transport: HTTP GET (7 Issues)**
# 

*
The APIs called in 7 Issues listed are all Apple's standard API. We could not change HTTP GET to HTTP POST. Although HTTP GET is less securable then HTTP POST, it could be used in most cases , image/documents downloading etc.  Fortify is too strict on this. Not issues, and need not to be fixed.
￼*
# 
# 
# 
#####  **     Key Management: Hardcoded Encryption Key (4 Issues)**
# 

*
The strings in the 4 issues listed is not Encryption Key, they are just plain texts,they need not to be Encrypted.Fortify mistakes them as password. Not issues, and need not to be fixed.
￼*

# 
# 
# 
#####  **Insecure Storage: Lacking Data Protection (2 Issues)**
# 

*
Buffer need not encrypt when downloading a image or documents.Not issues, and need not to be fixed.
￼*

# 
# 
# 
#####  ** Log Forging (2 Issues)**
# 

*
Not issues, The log is used for dubuging, and will be removed in release version.It is all right to remove the logs but not must. 
￼*

# 
# 
# 
#####  **  Insecure Storage: Missing Encryption on Stored Private Media (1 Issues)**
# 

*
Image saved to album need not to be encrypt. Not an issue, and need not to be fixed. 
￼*

# 
# 
# 
## Conclusion
# 
# 

>-*Fortify is too strict on some points. After review, we found that most issues listed need not to be fixed. [Signed to Unsigned] and [Log Forging] Category issues is all right but not must to fix ￼*


[Signed to Unsigned]:#
[Log Forging]:#
