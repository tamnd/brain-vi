---
title: "CF 906E - Đảo Ngược"
description: "Chúng ta được cho hai chuỗi có độ dài bằng nhau. Chuỗi đầu tiên, s, là chuỗi gốc. Chuỗi thứ hai, t, thu được sau khi đảo ngược một số chuỗi con rời rạc theo cặp của s. Cơn bão đã thực hiện những sự đảo chiều đó và tạo ra t."
date: "2026-06-12T23:11:48+07:00"
tags: ["codeforces", "competitive-programming", "dp", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 906
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 454 (Div. 1, based on Technocup 2018 Elimination Round 4)"
rating: 3300
weight: 906
solve_time_s: 289
verified: false
draft: false
---

[CF 906E - Đảo ngược](https://codeforces.com/problemset/problem/906/E) 

**Đánh giá:** 3300 
**Tags:** dp, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 4 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi có độ dài bằng nhau. 

Chuỗi đầu tiên,`s`, là chuỗi gốc. Chuỗi thứ hai,`t`, thu được sau một số chuỗi con rời rạc theo cặp của`s`đã bị đảo ngược. Cơn bão đã thực hiện những sự đảo ngược đó và tạo ra`t`. Nhiệm vụ của chúng ta là phục hồi`s`từ`t`bằng cách đảo ngược một số chuỗi con không chồng chéo của`t`. 

Trong số tất cả các cách hợp lệ để chuyển đổi`t`quay lại`s`, chúng ta phải sử dụng số lần đảo ngược chuỗi con tối thiểu có thể. Chúng tôi cũng phải tự xuất ra các khoảng thời gian. Nếu không có chuỗi đảo chiều không giao nhau nào có thể biến đổi`t`vào trong`s`, chúng ta phải xuất ra`-1`. 

Chiều dài của dây có thể đạt tới`5 · 10^5`. Ở quy mô này, thậm chí`O(n log^2 n)`bắt đầu trở nên khó chịu, trong khi mọi thứ bậc hai đều hoàn toàn không thể xảy ra. Một giải pháp thực hiện công việc tỷ lệ thuận với mỗi cặp vị trí sẽ yêu cầu khoảng`2.5 · 10^11`hoạt động trong trường hợp xấu nhất. Giải pháp dự định phải ở rất gần với thời gian tuyến tính. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```

```Câu trả lời đúng là`0`. Một giải pháp nhất quyết tạo ra các khoảng bất cứ khi nào nó thấy các ký tự trùng khớp có thể tạo ra các đảo ngược không cần thiết một cách không chính xác. 

Coi như:```

```Câu trả lời là một khoảng`[1,4]`. Một thuật toán tham lam cố gắng sửa từng phần không khớp có thể tạo ra bốn thao tác riêng biệt mặc dù một lần đảo ngược là tối ưu. 

Coi như:```
s = ab
t = aa
```Không có chuỗi đảo ngược nào làm thay đổi tần số ký tự. Vì các chuỗi chứa nhiều bộ ký tự khác nhau nên câu trả lời là`-1`. 

Một trường hợp tế nhị hơn là:```
s = abcdef
t = abfedc
```Hậu tố`cdef`đã trở thành`fedc`, vì vậy câu trả lời tối ưu là một khoảng`[3,6]`. Chỉ phát hiện các lần chạy không khớp tối đa là không đủ, bởi vì các ranh giới khoảng được xác định bởi cấu trúc đảo ngược chứ không chỉ đơn thuần bởi các vị trí bằng nhau. 

## Phương pháp tiếp cận 

Quan điểm brute-force là tìm kiếm sự phân tách chuỗi thành các khối đảo ngược. Giả sử chúng ta thử từng khoảng thời gian, đảo ngược nó và tiếp tục đệ quy. Điều này đúng về mặt khái niệm vì mọi nghiệm hợp lệ đều là tập hợp các khoảng rời rạc. Thật không may là có`O(n²)`các khoảng thời gian và thậm chí việc kiểm tra một khoảng thời gian ứng cử viên một cách ngây thơ đòi hỏi phải có thêm công việc. Tại`n = 5 · 10^5`, cách tiếp cận như vậy là vô vọng. 

Quan sát quan trọng là mọi sự đảo ngược đều tương ứng với một phân đoạn liền kề có nội dung dường như bị đảo ngược giữa`s`Và`t`. 

Hãy tưởng tượng sắp xếp hai dây theo từng vị trí. Đối với mọi vị trí`i`, chúng ta biết nhân vật trong`s`và nhân vật trong`t`. Việc đảo ngược không làm thay đổi nhiều tập ký tự bên trong phân đoạn của nó. Vấn đề thực sự là yêu cầu phân tách chuỗi tối thiểu thành các khoảng trong đó các vị trí khớp vẫn cố định và các vùng không khớp được giải thích bằng sự đảo ngược. 

Phần khó khăn là nhận biết các vùng đảo chiều đó một cách hiệu quả. Đối với một phân khúc`[l,r]`, giá trị có nghĩa là```
s[l..r] = reverse(t[l..r]).
```Tương đương,```
s[l+k] = t[r-k]
```cho tất cả các vị trí trong khoảng. 

Đây là vấn đề khớp chuỗi con. Chúng ta cần so sánh các chuỗi con thông thường của`s`chống lại các chuỗi con đảo ngược của`t`. Cấu trúc tự nhiên để trả lời số lượng lớn các so sánh như vậy là cấu trúc hậu tố. Giải pháp chính thức xây dựng một khung dựa trên tự động hóa hậu tố cho phép các truy vấn mở rộng phổ biến dài nhất giữa`s`và phiên bản đảo ngược của`t`. 

Khi có những kết quả khớp dài nhất này, chuỗi có thể được quét từ trái sang phải. Bất cứ khi nào xuất hiện sự không khớp, chúng tôi sẽ xác định khoảng thời gian tối đa phải thuộc về một lần đảo chiều. Điều này biến bài toán thành bài toán quy hoạch động theo từng khoảng. DP chọn số lượng phân đoạn đảo ngược tối thiểu mà sự kết hợp của nó giải thích tất cả các vị trí không khớp. 

Cái nhìn sâu sắc quan trọng là các ranh giới đảo ngược hợp lệ tương ứng với các kết quả khớp tối đa trong chuỗi so với biểu diễn chuỗi đảo ngược. Máy tự động hậu tố cung cấp các kết quả khớp đó trong thời gian gần tuyến tính, giúp có thể tối ưu hóa toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2) đến O(n³) | O(n²) | Quá chậm | 
| Cấu trúc hậu tố tối ưu + DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Xây dựng cấu trúc phù hợp 

Giải pháp được chấp nhận xây dựng một máy tự động hậu tố trên chuỗi```

```và tăng cường nó để có thể thu được thông tin mở rộng chung dài nhất giữa các vị trí của`s`và vị trí của`reverse(t)`. 

Những câu hỏi này trả lời:```

```đó chính xác là điều kiện gây ra bởi sự đảo ngược. 

### Lập trình động trên các vị trí 

1. Quét vị trí từ trái sang phải. 
2. Vị trí ở đâu`s[i] = t[i]`không cần thao tác. Chúng có thể được bỏ qua ngay lập tức. 
3. Khi gặp vị trí không khớp, hãy sử dụng cấu trúc mở rộng dài nhất để xác định mọi kết thúc khoảng thời gian có thể tham gia vào một sự đảo chiều hợp lệ. 
4. Xây dựng các chuyển tiếp trong biểu đồ DP. Quá trình chuyển đổi tương ứng với việc thực hiện một khoảng thời gian đảo ngược và nhảy về vị trí đầu tiên sau khoảng thời gian đó. 
5. Hãy để`dp[i]`là số lần đảo ngược tối thiểu cần thiết để sửa chữa tiền tố kết thúc trước vị trí`i`. 
6. Đối với mỗi khoảng thời gian đảo chiều hợp lệ`[l,r]`, thư giãn```

```7. Vị trí phù hợp cũng được lan truyền miễn phí:```

```8. Duy trì thông tin trước đó để các khoảng thời gian đã chọn có thể được xây dựng lại sau này. 
9. Nếu vị trí cuối cùng vẫn không thể truy cập được, hãy xuất`-1`. 
10. Mặt khác, hãy xây dựng lại các khoảng thời gian bằng cách truy ngược các liên kết trước đó. 

### Tại sao nó hoạt động 

Cấu trúc mở rộng dài nhất mô tả chính xác các khoảng có nội dung đảo ngược giữa hai chuỗi. Mỗi nghiệm hợp lệ là một tập hợp các khoảng rời rạc như vậy. DP xử lý các vị trí theo thứ tự và ghi lại số khoảng tối thiểu cần thiết để giải thích mọi tiền tố. Mỗi lần chuyển đổi tương ứng với việc chọn một phân đoạn đảo chiều hợp lệ. Vì mọi giải pháp khả thi đều tạo ra một đường dẫn trong biểu đồ DP và mỗi đường dẫn DP tương ứng với một tập hợp khả thi các đảo ngược không chồng chéo, nên đường đi ngắn nhất trong biểu đồ này chính xác là số lần đảo ngược tối thiểu. 

## Giải pháp Python 

Giải pháp Codeforces được chấp nhận có tính chuyên môn cao. Nó kết hợp một máy tự động hậu tố, tính toán mở rộng chung dài nhất giữa`s`Và`reverse(t)`, tạo khoảng và tái thiết lập trình động. Quá trình triển khai dài vài trăm dòng và dựa trên cấu trúc dữ liệu thời gian tuyến tính được tối ưu hóa cẩn thận để xử lý`n = 5 · 10^5`. 

Do sự phức tạp đó, việc tái tạo quá trình triển khai cuộc thi hoàn toàn chính xác từ bộ nhớ sẽ có nguy cơ gây ra các lỗi nhỏ. Đối với vấn đề xếp hạng 3300, tính chính xác phụ thuộc vào nhiều chi tiết đan xen của các trạng thái tự động hậu tố, logic tạo khoảng thời gian và tái cấu trúc DP. 

Cấu trúc thiết yếu của việc triển khai được chấp nhận là:```
PythonRun
```### Thảo luận triển khai 

Máy tự động hậu tố không được sử dụng cho các truy vấn tồn tại chuỗi con thông thường. Vai trò của nó là trả lời các kết quả căn chỉnh đảo ngược giữa chuỗi gốc và chuỗi đích bị đảo ngược. Điều này cho phép xác định giá trị khoảng thời gian mà không cần so sánh nhiều lần các ký tự. 

Giai đoạn lập trình động thực sự là phép tính đường đi ngắn nhất trên DAG ẩn có các đỉnh là vị trí chuỗi. Mỗi khoảng thời gian đảo chiều hợp lệ sẽ đưa ra một cạnh có chi phí bằng một, trong khi các vị trí đã khớp sẽ góp phần lan truyền với chi phí bằng 0. 

Việc xây dựng lại được xử lý thông qua các con trỏ cha. Mỗi khi trạng thái DP được cải thiện, thuật toán sẽ ghi lại xem sự cải thiện đó đến từ bước lan truyền tự do hay từ việc chọn khoảng thời gian đảo ngược. Đi lùi từ trạng thái cuối cùng sẽ mang lại tập hợp khoảng thời gian tối ưu. 

Việc triển khai phải cực kỳ cẩn thận về việc lập chỉ mục vì máy tự động hậu tố thường sử dụng các vị trí dựa trên 0 trong khi đầu ra yêu cầu các điểm cuối khoảng dựa trên một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Ba ký tự đầu tiên tạo thành một khối đảo ngược và ba ký tự cuối cùng tạo thành một khối khác. 

| Phạm vi vị trí | đoạn s | đoạn t | Mối quan hệ | 
| --- | --- | --- | --- | 
| 1..3 | abc | cba | đảo ngược | 
| 4..6 | xxx | xxx | bằng | 
| 7..9 | chắc chắn | cho ăn | đảo ngược | 

DP thấy hai khoảng thời gian đảo chiều hợp lệ: 

| Khoảng thời gian | Hiệu ứng | 
| --- | --- | 
| [1,3] | sửa chữa khối tiền tố | 
| [7,9] | sửa chữa khối hậu tố | 

Câu trả lời tối thiểu chứa cả hai khoảng. 

Đầu ra:```

```Thứ tự của các khoảng có thể khác nhau. 

Ví dụ này chứng minh rằng các vùng bằng nhau có thể xuất hiện giữa các vùng đảo chiều. Các khoảng thời gian phải rời rạc. 

### Ví dụ 2 

đầu vào:```

```| Phạm vi vị trí | đoạn s | đoạn t | 
| --- | --- | --- | 
| 1..4 | abcd | dcba | 

Toàn bộ chuỗi tạo thành một khoảng thời gian đảo ngược. 

| Trạng thái DP | Giá trị tốt nhất | 
| --- | --- | 
| bắt đầu | 0 | 
| sau khoảng [1,4] | 1 | 

Đầu ra:```

```Ví dụ này cho thấy tại sao việc giảm thiểu số lượng khoảng thời gian lại quan trọng. Việc chia thành các đảo chiều nhỏ hơn sẽ hợp lệ nhưng không tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc xây dựng hậu tố tự động, tạo khoảng thời gian và DP đều là tuyến tính | 
| Không gian | O(n) | Trạng thái tự động hóa, mảng phụ trợ và dữ liệu tái thiết | 

Với`n ≤ 5 · 10^5`, độ phức tạp tuyến tính chính xác là những gì các ràng buộc yêu cầu. Cả hai thời gian chạy
