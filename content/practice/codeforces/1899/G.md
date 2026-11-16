---
title: "CF 1899G - Giải Trí Khác Thường"
description: "Chúng ta có một cây có gốc có gốc ở đỉnh 1. Hoán vị p chứa mỗi đỉnh đúng một lần. Mỗi truy vấn đưa ra ba giá trị (l, r, x). Chúng ta xét các đỉnh xuất hiện trong đoạn hoán vị p[l..."
date: "2026-06-08T21:26:49+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dsu", "shortest-paths", "sortings", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1899
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 909 (Div. 3)"
rating: 1900
weight: 1899
solve_time_s: 46
verified: false
draft: false
---

[CF 1899G - Giải trí khác thường](https://codeforces.com/problemset/problem/1899/G) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dsu, đường dẫn ngắn nhất, sắp xếp, cây, hai con trỏ 
**Thời gian giải:** 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc ở đỉnh`1`. Một hoán vị`p`chứa mỗi đỉnh đúng một lần. 

Mỗi truy vấn đưa ra ba giá trị`(l, r, x)`. Chúng ta xét các đỉnh xuất hiện trong đoạn hoán vị`p[l...r]`và hỏi xem ít nhất một trong các đỉnh đó có thuộc cây con của`x`. 

Vì cây đã có rễ nên “con cháu của`x`" hoàn toàn giống với "đỉnh bên trong cây con của`x`", bao gồm`x`chính nó. 

Được diễn đạt lại, mọi truy vấn đều hỏi: 

> Trong số các đỉnh có vị trí hoán vị nằm giữa`l`Và`r`, liệu có tồn tại một đỉnh bên trong cây con của`x`? 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`, và tổng số truy vấn cũng nhiều nhất`10^5`. Bất kỳ giải pháp nào quét toàn bộ cây con hoặc toàn bộ khoảng hoán vị cho mỗi truy vấn đều sẽ quá chậm. Một cách tiếp cận bậc hai có thể dễ dàng yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất. 

Thách thức là truy vấn kết hợp hai cấu trúc khác nhau. 

Điều kiện cây con tồn tại trong cây. 

Điều kiện khoảng tồn tại ở các vị trí hoán vị. 

Giải pháp phải kết hợp hiệu quả cả hai. 

Hãy xem xét một chuỗi`1-2-3`.```
p = [1,2,3]
query = (1,2,3)
```Khoảng chứa các đỉnh`{1,2}`. 

Cây con của`3`chỉ chứa`{3}`. 

Câu trả lời là`NO`. 

Việc thực hiện bất cẩn để kiểm tra xem`x`chính nó xuất hiện trong khoảng sẽ trả lời sai`YES`cho nhiều trường hợp tương tự. 

Coi như:```
1
3 1
1 2
1 3
2 1 3
1 1 1
```Khoảng chỉ chứa đỉnh`2`. 

Cây con của`1`chứa mọi đỉnh. 

Câu trả lời là`YES`. 

Bất kỳ cách tiếp cận nào chỉ kiểm tra con trực tiếp của`1`sẽ thất bại vì con cháu không bị giới hạn ở độ sâu một. 

Một trường hợp tinh vi khác xảy ra khi đỉnh trùng khớp chính xác`x`.```
1
1 1
1
1 1 1
```Cây con của`1`chứa`1`chính nó, vì vậy câu trả lời là`YES`. 

Việc triển khai coi con cháu là con cháu nghiêm ngặt sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp rất dễ mô tả. Đối với một truy vấn`(l,r,x)`, kiểm tra mọi đỉnh trong`p[l...r]`và kiểm tra xem nó có thuộc cây con của`x`. 

Sử dụng dấu thời gian tham quan Euler, có thể đăng ký tư cách thành viên cây con`O(1)`, nhưng độ dài khoảng có thể là`O(n)`. Một truy vấn có thể yêu cầu quét gần như toàn bộ hoán vị. Với`10^5`truy vấn, điều này trở thành`O(nq)`, quá lớn. 

Quan sát quan trọng là cây con trở thành một khoảng liền kề theo thứ tự chuyến tham quan Euler. 

Giả sử chúng ta chạy DFS và chỉ định thời gian nhập`tin[v]`. 

Mỗi đỉnh trong cây con của`x`nhận được dấu thời gian bên trong:```
[tin[x], tout[x]]
```Bây giờ hãy nhìn vào hoán vị. Đối với mọi vị trí`i`, thay vì lưu trữ đỉnh`p[i]`, lưu trữ thời gian vào Euler của nó:```
a[i] = tin[p[i]]
```Một truy vấn hỏi liệu một số đỉnh của cây con của`x`xuất hiện giữa các vị trí`l`Và`r`. 

Sau khi chuyển đổi, truy vấn sẽ trở thành: 

> Phân đoạn mảng`a[l...r]`chứa một giá trị bên trong khoảng`[tin[x], tout[x]]`? 

Đây bây giờ là một vấn đề hình học thuần túy liên quan đến vị trí mảng và phạm vi giá trị. 

Một kỹ thuật ngoại tuyến rất hữu ích cho các truy vấn tồn tại như vậy là xử lý các vị trí mảng từ trái sang phải trong khi duy trì, đối với mỗi giá trị Euler, vị trí mới nhất mà nó xuất hiện. 

Đối với một truy vấn`(l,r,x)`, chúng tôi muốn biết liệu một số giá trị Euler có thuộc khoảng cây con của`x`xuất hiện ở những vị trí`[l,r]`. 

Chúng tôi xử lý các truy vấn được nhóm theo điểm cuối bên phải của chúng`r`. 

Khi vị trí`r`được xử lý, tất cả các giá trị cho đến thời điểm đó đã được inse
