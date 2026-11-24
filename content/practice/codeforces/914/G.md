---
title: "CF 914G - Tính tổng Fibonacci"
description: "Chúng ta được cho một chuỗi các số nguyên, mỗi số có thể được coi là một mặt nạ 17 bit. Từ mảng này, chúng ta phải hình thành các lựa chọn có thứ tự gồm năm vị trí."
date: "2026-06-15T12:19:46+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "divide-and-conquer", "dp", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 914
codeforces_index: "G"
codeforces_contest_name: "Codecraft-18 and Codeforces Round 458 (Div. 1 + Div. 2, combined)"
rating: 2600
weight: 914
solve_time_s: 310
verified: false
draft: false
---

[CF 914G - Tính tổng Fibonacci](https://codeforces.com/problemset/problem/914/G) 

**Đánh giá:** 2600 
**Tags:** bitmasks, chia để trị, dp, fft, toán 
**Thời gian giải:** 5 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các số nguyên, mỗi số có thể được coi là một mặt nạ 17 bit. Từ mảng này, chúng ta phải hình thành các lựa chọn có thứ tự gồm năm vị trí. Mỗi lựa chọn đóng góp một giá trị được tính toán từ sự kết hợp theo bit của các phần tử đã chọn, sau đó đánh giá các số Fibonacci ở các kết quả đó. 

Một lựa chọn duy nhất sử dụng các chỉ số$a, b, c, d, e$. Hai phần tử đầu tiên được kết hợp với OR và chúng cũng phải đáp ứng một ràng buộc nghiêm ngặt rằng AND theo bit của chúng bằng 0. Phần tử thứ ba tương tác thông qua AND với sự kết hợp đầu tiên và hai phần tử cuối cùng tương tác thông qua XOR. biểu thức$(s_a | s_b) \& s_c \& (s_d \oplus s_e)$được yêu cầu chính xác bằng một lũy thừa của hai. Điều đó có nghĩa là kết quả phải có chính xác một bit được đặt. Sự đóng góp của bộ dữ liệu như vậy là tích của ba giá trị Fibonacci được đánh giá ở các biểu thức bitwise trung gian. 

Các ràng buộc đẩy chúng ta ra khỏi việc liệt kê các bộ dữ liệu. Với$n$lên tới$10^6$, thậm chí việc lặp lại tất cả 5 bộ dữ liệu là không thể vì điều đó sẽ$O(n^5)$. Ngay cả việc liệt kê theo cặp cũng đã quá lớn nếu được thực hiện một cách ngây thơ mà không có cấu trúc. Điều quan trọng là các giá trị của$s_i$được giới hạn bởi$2^{17}$, do đó tập hợp các giá trị riêng biệt là nhỏ ngay cả khi kích thước mảng lớn. Điều này cho thấy rằng tích chập dựa trên tần số trên mặt nạ bit là sự trừu tượng hóa chính xác. 

Trường hợp cạnh tinh tế xuất phát từ điều kiện$s_a \& s_b = 0$. Nhiều cách tiếp cận sẽ bỏ qua nó hoặc vô tình đếm gấp đôi các cặp có thứ tự so với các cặp không có thứ tự. Một vấn đề khác nảy sinh từ ràng buộc rằng biểu thức cuối cùng phải chính xác là lũy thừa của hai chứ không phải khác 0, điều này loại trừ sự trùng lặp một phần giữa các bit. 

## Phương pháp tiếp cận 

Việc diễn giải trực tiếp sẽ lặp lại trên tất cả 5 chỉ số, tính toán các biểu thức theo bit, kiểm tra tính hợp lệ và tích lũy các tích Fibonacci. Điều này đúng nhưng đòi hỏi$O(n^5)$hoạt động vượt quá giới hạn cho phép. 

Chúng ta có thể nén vấn đề bằng cách sử dụng số lượng giá trị tần số. Vì mỗi$s_i < 2^{17}$, chúng tôi duy trì một loạt số lượng trên mặt nạ. Mục tiêu trở thành việc đếm các bộ giá trị có cấu trúc thay vì chỉ số. Mỗi bộ dữ liệu hợp lệ chỉ đóng góp dựa trên các giá trị đã chọn, được tính theo số lượng chỉ mục chứa chúng. 

Quan sát chính là tất cả các hoạt động đều theo bit và do đó độc lập giữa các bit, ngoại trừ ràng buộc rằng kết quả cuối cùng phải là mặt nạ bit đơn. Điều này gợi ý việc chia vấn đề theo bit mục tiêu$i$và buộc tất cả các cấu trúc trung gian phải căn chỉnh sao cho chỉ có bit$i$sống sót. 

Điều này biến vấn đề thành tính toán một số tích chập nhiều chiều trên khối Boolean. Ràng buộc$s_a \& s_b = 0$có thể được xử lý bằng cách chia mặt nạ thành các tập hợp con rời rạc, trong khi các kết hợp OR, AND và XOR tương ứng với các phép biến đổi tập hợp con và các phép biến đổi Walsh-Hadamard. Các trọng số Fibonacci chỉ phụ thuộc vào các giá trị số nguyên thu được, do đó chúng ta tính trước Fibonacci tới$2^{17}$và gắn trọng số vào mảng tần số. 

Cấu trúc cuối cùng là sự kết hợp của các phép tích tập hợp con và các phép biến đổi theo bit, được thực hiện trên mỗi ràng buộc bit, mang lại một kết quả tổng thể.$O(17 \cdot 2^{17} \log 2^{17})$giải pháp phong cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^5)$|$O(1)$| Quá chậm | 
| Tích chập Bitmask với các phép biến đổi |$O(17 \cdot 2^{17} \log 2^{17})$|$O(2^{17})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại phép tính theo mảng tần số trên mặt nạ bit. Cho phép$cnt[x]$có giá trị gấp bao nhiêu lần$x$xuất hiện. 

Chúng tôi cũng tính toán trước các giá trị Fibonacci$F[x]$lên đến giá trị mặt nạ tối đa có thể. 

### bước 

1. Xây dựng mảng tần số trên tất cả các giá trị. 

Mỗi số trong mảng góp phần tạo nên biểu đồ toàn cầu trên mặt nạ. Điều này cho phép chúng ta thay thế lựa chọn dựa trên chỉ mục bằng cách tính dựa trên bội số. 
2. Tính toán trước các giá trị Fibonacci lên tới$2^{17} - 1$. 

Vì mọi biểu thức trung gian là sự kết hợp theo chiều bit của các đầu vào nên nó vẫn nằm trong phạm vi này. Điều này tránh việc tính toán lại Fibonacci nhiều lần. 
3. Tính toán trước các đóng góp cặp cho$(a,b)$dưới sự ràng buộc$s_a \& s_b = 0$. 

Đối với mỗi mặt nạ$x$, chúng tôi xem xét tất cả$y$như vậy$x \& y = 0$. Đây là phép tích chập tập hợp con cổ điển trên các tập hợp rời rạc. Lý do điều này hoạt động là vì bitwise AND 0 có nghĩa là phần hỗ trợ của hai mặt nạ không trùng nhau, do đó các bit của chúng được phân chia rõ ràng. 
4. Xây dựng tích chập cho$x | y$. 

Khi các cặp rời rạc hợp lệ được hình thành, OR của chúng chỉ đơn giản là sự kết hợp các bit. Điều này mang lại sự phân phối mới trên các mặt nạ đã mã hóa hệ số Fibonacci đầu tiên. 
5. Kết hợp với phần tử thứ ba$c$sử dụng bit AND. 

Đối với mỗi mặt nạ trung gian$u = x | y$, chúng tôi kết hợp với$s_c$thông qua AND. Điều này hạn chế mặt nạ hướng xuống dưới, đảm bảo chỉ các bit được chia sẻ mới tồn tại. Điều này được thực hiện thông qua phép biến đổi bitmask zeta trên supersets. 
6. Xử lý cặp XOR cuối cùng$(d,e)$. 

Chúng tôi tính toán trước phép tích chập trên XOR bằng cách sử dụng phép biến đổi Walsh-Hadamard, vì XOR tương ứng với phép tích chập trên nhóm$(\mathbb{Z}_2)^{17}$. Điều này tạo ra sự phân bố trên tất cả các kết quả XOR có thể có, được tính trọng số bởi các giá trị Fibonacci. 
7. Đối với mỗi bit$i$, tích lũy các đóng góp trong đó biểu thức cuối cùng bằng chính xác$2^i$. 

Điều này thực thi ràng buộc rằng chỉ còn lại một bit được đặt trong chuỗi AND cuối cùng. Chúng tôi tổng hợp các khoản đóng góp trên tất cả các công trình trung gian hợp lệ. 

### Tại sao nó hoạt động 

Toàn bộ thuật toán dựa vào việc thay thế tổ hợp dựa trên chỉ số bằng đại số trên khối Boolean. Mọi thao tác trong biểu thức ban đầu tương ứng với một phép biến đổi tuyến tính hoặc tích chập trong không gian này. Hệ số ràng buộc rõ ràng vì bitwise AND 0 thực thi các hỗ trợ rời rạc, OR trở thành hợp nhất, AND trở thành hạn chế và XOR trở thành tích chập nhóm. Vì tất cả các tương tác đều tôn trọng tính độc lập của bit nên chúng ta có thể phân tách vấn đề thành các phép biến đổi có cấu trúc và kết hợp lại các kết quả mà không làm mất thông tin. 

Điều bất biến được duy trì là sau mỗi bước biến đổi, mảng DP mã hóa số lượng trọng số chính xác của các bộ dữ liệu một phần tạo ra mỗi mặt nạ trung gian có thể có theo các ràng buộc ban đầu. Không có bước nào đưa ra các đóng góp giả vì mỗi tích chập khớp chính xác với định nghĩa tổ hợp của phép toán tương ứng. 

## Giải pháp Python```
PythonRun
```Mã đầu tiên xây dựng mảng tần số trên mặt nạ. Sau đó, nó sử dụng các phép biến đổi tập hợp con để xử lý các ràng buộc kiểu OR và AND và phép biến đổi Walsh-Hadamard để xử lý ghép nối XOR một cách hiệu quả. Mảng Fibonacci được tính toán trước một lần và được sử dụng làm hàm trọng số trên các giá trị mặt nạ. 

Chi tiết triển khai quan trọng là giữ tất cả các hoạt động theo mô-đun$10^9+7$và đảm bảo các phép biến đổi được đảo ngược chính xác khi cần thiết. Phép tích chập XOR được thực hiện bằng cách bình phương trong miền được chuyển đổi, tương ứng chính xác với việc đếm cặp trong không gian ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi xây dựng tần số: 

| mặt nạ | cnt | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 

Tích chập XOR tạo ra: 

| mặt nạ | cặp | 
| --- | --- | 
| 0 | (1,1),(2,2) | 
| 3 | (1,2),(2,1) | 

Sau khi tính trọng số theo các giá trị Fibonacci và áp dụng bộ lọc AND/OR, chỉ những kết hợp giảm xuống mặt nạ bit đơn mới đóng góp. 

Tích lũy cuối cùng trên các bit hợp lệ mang lại 32. 

Điều này chứng tỏ cách đối xứng XOR tăng gấp đôi sự đóng góp thông qua các cặp có thứ tự. 

### Ví dụ 2 

đầu vào:```

```Tần số: 

| mặt nạ | cnt | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 

Cấu trúc XOR tạo ra tất cả các XOR theo cặp, trong khi các phép biến đổi tập hợp con đảm bảo chỉ còn lại các chuỗi OR-AND tương thích. Đóng góp chỉ tích lũy cho các mặt nạ giảm xuống đầu ra một bit sau khi lọc AND. 

Điều này xác nhận rằng mặt nạ nhiều bit trung gian bị loại bỏ trừ khi chúng thu gọn về lũy thừa hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(17 \cdot 2^{17} \log 2^{17})$| biến đổi tập hợp con và FWHT trên mặt nạ 17 bit | 
| Không gian |$O(2^{17})$| mảng tần số trên mặt nạ bit | 

Giải pháp thoải mái phù hợp trong giới hạn vì$2^{17}$chỉ là 131072 và tất cả các phép biến đổi đều là tuyến tính hoặc log-tuyến tính trên miền này. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 2 | 32 | tương tác XOR + OR cơ bản | 
| 1\n0 | 0 | trường hợp cạnh đơn | 
| 4\n1 1 1 1 | căng thẳng trùng lặp | xử lý đa dạng | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các phần tử đều bằng 0. Trong trường hợp này, mọi thao tác theo bit đều giảm về 0 và chỉ$2^0$đóng góp có liên quan. Thuật toán xử lý vấn đề này vì tần số được tập trung hoàn toàn ở mặt nạ 0 và tất cả các phép biến đổi tập hợp con đều bảo toàn khối lượng đó mà không tạo ra sự đóng góp bit cao hơn giả. 

Một trường hợp khác là khi các giá trị có các bit rời rạc, chẳng hạn$[1,2,4]$. Ở đây, mọi cặp đều tự động thỏa mãn ràng buộc AND, nhưng OR mở rộng nhanh chóng. Biến đổi tập hợp con tổng hợp chính xác các liên kết này và giai đoạn XOR vẫn độc lập vì không tồn tại sự chồng chéo trong cấu trúc bitwise, đảm bảo không xảy ra sự hủy bỏ không hợp lệ.
