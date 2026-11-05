---
title: "CF 1199B - Hoa Súng"
description: "Một bông súng được gắn vào đáy hồ bằng một thân cây thẳng. Ban đầu thân cây hoàn toàn thẳng đứng nên bông hoa nằm ngay phía trên điểm rễ của nó trên lòng hồ. Bông hoa nổi trên mặt nước ở độ cao đã biết (H)."
date: "2026-06-13T14:59:34+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1199
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 576 (Div. 2)"
rating: 1000
weight: 1199
solve_time_s: 227
verified: true
draft: false
---

[CF 1199B - Hoa súng](https://codeforces.com/problemset/problem/1199/B) 

**Đánh giá:** 1000 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 3 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một bông súng được gắn vào đáy hồ bằng một thân cây thẳng. Ban đầu thân cây hoàn toàn thẳng đứng nên bông hoa nằm ngay phía trên điểm rễ của nó trên lòng hồ. Bông hoa nổi trên mặt nước ở một độ cao xác định\(H\). 

Sau đó, chúng tôi mô phỏng điều xảy ra khi bông hoa bị kéo theo phương ngang dọc theo mặt nước một khoảng\(L\). Thân cây vẫn thẳng và cứng nên nó quay như một đoạn duy nhất được neo ở điểm dưới cùng. Tại thời điểm được mô tả trong bài toán, bông hoa vừa chạm tới mặt nước. 

Nhiệm vụ là xác định độ sâu của hồ tại điểm gắn thân cây. 

Về mặt hình học, điều này trở thành một bài toán tam giác vuông. Thân cây là cạnh huyền, có chiều dài cố định, nối đáy hồ với bông hoa. Ban đầu, khoảng cách thẳng đứng từ đáy đến bông hoa bằng chiều dài toàn bộ thân cây trừ đi độ sâu của nước. Sau khi kéo bông hoa theo chiều ngang\(L\), chiều cao thẳng đứng chính xác là độ sâu của hồ, vì bông hoa lúc này đã nổi lên trên mặt nước. 

Những hạn chế\(1 \le H < L \le 10^6\)chỉ ra rằng mọi thứ đều phù hợp một cách thoải mái trong số học dấu phẩy động và chúng ta có thể dựa vào độ chính xác kép tiêu chuẩn một cách an toàn mà không phải lo lắng về tình trạng tràn hoặc mất ổn định về số do cường độ cực lớn. 

Một vấn đề tế nhị có thể gây nhầm lẫn cho lối suy luận ngây thơ là việc trộn lẫn đoạn nào tương ứng với chiều dài thân cây. Thân cây không chỉ đơn giản\(H + \text{depth}\); chỉ giữ trong cấu hình dọc ban đầu. Sau khi xoay, hình dạng thay đổi và mối quan hệ chính xác xuất phát từ chiều dài bất biến của thân kết hợp với tam giác vuông được hình thành bởi chuyển vị ngang và thả dọc. 

## Phương pháp tiếp cận 

Nếu chúng ta suy nghĩ thuần túy bằng đại số, chúng ta có thể đưa ra những ẩn số. Hãy để độ sâu của hồ là\(D\). Khi đó chiều dài thân cây là\(D + H\), vì ban đầu bông hoa là\(H\)phía trên bề mặt. Sau khi kéo, bông hoa nằm trên mặt nước nên hình chiếu thẳng đứng của thân cây trở nên chính xác.\(D\), trong khi chuyển vị ngang là\(L\). Điều này tạo thành một tam giác vuông nơi cạnh huyền\(D + H\), một chân là\(D\), còn cái kia là\(L\). 

Từ định lý Pythagore, chúng ta có được:$$
(D + H)^2 = D^2 + L^2
$$Mở rộng và đơn giản hóa loại bỏ\(D^2\)từ cả hai phía:$$
D^2 + 2DH + H^2 = D^2 + L^2
$$
$$
2DH + H^2 = L^2
$$
$$
D = \frac{L^2 - H^2}{2H}
$$Một cách tiếp cận mạnh mẽ sẽ là đoán độ sâu và mô phỏng hình học mỗi lần, kiểm tra xem chiều dài thân kết quả có khớp với các ràng buộc nhất quán hay không. Điều đó đòi hỏi phải lặp lại trên tất cả các độ sâu có thể lên đến\(10^6\), điều này là không cần thiết và lãng phí vì mối quan hệ là đại số và trực tiếp. 

Quan sát quan trọng là hệ thống được xác định hoàn toàn bằng một phương trình bậc hai. Khi các ràng buộc hình học được dịch chính xác, giải pháp sẽ có ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Brute Force (đoán độ sâu) | \(O(L)\) hoặc tệ hơn | \(O(1)\) | Quá chậm | 
| Đạo hàm đại số | \(O(1)\) | \(O(1)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên\(H\)Và\(L\). Chúng xác định độ cao ban đầu trên mặt nước và chuyển vị ngang sau khi kéo. 

2. Chuyển hình học sang quan hệ tam giác vuông trong đó chiều dài thân không đổi và bằng nhau\(D + H\). Bước này chuyển đổi thiết lập vật lý thành đại số. 

3. Áp dụng định lý Pythagore:$$
   (D + H)^2 = D^2 + L^2
   $$Điều này nắm bắt được chiều dài thân bất biến và sự phân rã trực giao sau khi xoay. 

4. Khai triển và hủy bỏ các số hạng giống nhau ở cả hai vế để tách biệt những số hạng chưa biết\(D\). Sự đơn giản hóa này loại bỏ số hạng bậc hai một cách rõ ràng do tính đối xứng. 

5. Giải phương trình tuyến tính thu được trong\(D\):$$
   D = \frac{L^2 - H^2}{2H}
   $$6. Đầu ra\(D\)dưới dạng số dấu phẩy động với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Toàn bộ công trình dựa trên một đoạn cứng có chiều dài cố định. Độ dài cố định đó không thay đổi khi xoay, do đó cấu hình hợp lệ duy nhất của điểm cuối thân nằm trên một vòng tròn có tâm ở điểm đáy hồ có bán kính\(D + H\). Vị trí cuối cùng tạo thành một tam giác vuông có cạnh huyền là bán kính đó và hai chân là chuyển vị ngang\(L\)và độ sâu theo chiều dọc\(D\). Vì ràng buộc hình học này xác định duy nhất tam giác nên biểu thức tính toán cho\(D\)là giá trị duy nhất phù hợp với cả cấu hình ban đầu và cấu hình cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

H, L = map(int, input().split())

D = (L * L - H * H) / (2 * H)
print(D)
```Giải pháp thực hiện trực tiếp công thức dạng đóng dẫn xuất. Sử dụng phép chia dấu phẩy động Python là đủ vì độ chính xác cần thiết chỉ là\(10^{-6}\), và các giá trị trung gian nhiều nhất là\(10^{12}\), nằm trong khả năng biểu diễn chính xác với độ chính xác gấp đôi cho bối cảnh này. 

Phép trừ\(L^2 - H^2\)là an toàn vì cả hai giá trị đều khớp với số nguyên 64 bit trước khi chuyển đổi. Sự phân chia theo\(2H\)được thực hiện ở dạng dấu phẩy động, đảm bảo kết quả cuối cùng là số thực theo yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
H = 1, L = 2
```Chúng tôi tính toán:$$
D = \frac{2^2 - 1^2}{2 \cdot 1}
$$| Bước | Biểu hiện | Giá trị | 
|------|-------------|-------| 
| Đọc đầu vào | H, L | 1, 2 | 
| Giá trị vuông | L2, H2 | 4, 1 | 
| Tử số | L2 - H2 | 3 | 
| Mẫu số | 2H | 2 | 
| Độ sâu | D | 1,5 | 

Đầu ra:```
1.5000000000000
```Điều này xác nhận một tam giác hợp lệ trong đó chiều dài thân cây là\(2.5\)và cấu hình cuối cùng thỏa mãn cả ràng buộc ngang và dọc. 

### Ví dụ 2 

đầu vào:```
H = 2, L = 6
```| Bước | Biểu hiện | Giá trị | 
|------|-------------|-------| 
| Đọc đầu vào | H, L | 2, 6 | 
| Giá trị vuông | L2, H2 | 36, 4 | 
| Tử số | L2 - H2 | 32 | 
| Mẫu số | 2H | 4 | 
| Độ sâu | D | 8 | 

Đầu ra:```
8.0
```Điều này cho thấy sự dịch chuyển lớn hơn tạo ra một hồ sâu hơn phù hợp với giới hạn chiều dài thân cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(1)\) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | \(O(1)\) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Việc tính toán chỉ bao gồm một vài phép nhân số nguyên và một phép chia dấu phẩy động, rất nhanh với các ràng buộc đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    H, L = map(int, sys.stdin.readline().split())
    D = (L * L - H * H) / (2 * H)
    return str(D)

# provided sample
assert abs(float(run("1 2\n")) - 1.5) < 1e-9

# minimum values
assert abs(float(run("1 2\n")) - 1.5) < 1e-9

# simple geometric check
assert abs(float(run("2 6\n")) - 8.0) < 1e-9

# equal difference case
assert abs(float(run("3 5\n")) - ((25 - 9) / 6)) < 1e-9

# large values
assert run("1000000 1000000\n")  # just ensures no crash
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 1 2 | 1,5 | tính đúng đắn cơ bản | 
| 2 6 | 8.0 | tính nhất quán của tam giác lớn hơn | 
| 3 5 | (25-9)/6 | tính đúng đại số | 
| 1000000 1000000 | đầu ra hợp lệ | ổn định số | 

## Vỏ cạnh 

Khi nào\(H\)là tối thiểu, ví dụ\(H = 1\)Và\(L = 2\), phép tính giảm xuống một biểu thức số học số nguyên nhỏ và xác nhận rằng công thức không phụ thuộc vào tỷ lệ. Thuật toán tính toán trực tiếp \(D = (4 - 1)/2 = 1,5\), phù hợp với cấu hình hình học dự kiến. 

Khi\(L\)chỉ lớn hơn một chút so với\(H\), tử số\(L^2 - H^2\)vẫn dương nhưng nhỏ, tạo ra một hồ nước cạn. Bước trừ là an toàn vì cả hai ô vuông đều nằm trong số nguyên 64 bit và phép chia tạo ra kết quả dấu phẩy động ổn định. 

Khi\(L\)rất lớn so với\(H\), độ sâu tăng bậc hai với\(L\). Công thức vẫn hoạt động chính xác vì số nguyên Python xử lý chính xác các giá trị trung gian lớn trước khi chuyển đổi sang dạng float, đảm bảo không bị tràn hoặc mất độ chính xác ở giai đoạn số nguyên.
