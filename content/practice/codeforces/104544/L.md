---
title: "CF 104544L - Quái Vật Máy Giặt"
description: "Chúng ta được phát một số chiếc tất và được yêu cầu xác định xem có thể tạo thành bao nhiêu đôi tất hoàn chỉnh. Một đôi bao gồm chính xác hai chiếc tất, vì vậy nhiệm vụ đơn giản là nhóm tất thành các nhóm rời rạc có cỡ hai và đếm xem có thể tạo được bao nhiêu nhóm như vậy."
date: "2026-06-30T09:07:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104544
codeforces_index: "L"
codeforces_contest_name: "Aleppo Collegiate Programming Contest 2023 V.2"
rating: 0
weight: 104544
solve_time_s: 58
verified: true
draft: false
---

[CF 104544L - Quái vật máy giặt](https://codeforces.com/problemset/problem/104544/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một số chiếc tất và được yêu cầu xác định xem có thể tạo thành bao nhiêu đôi tất hoàn chỉnh. Một đôi bao gồm chính xác hai chiếc tất, vì vậy nhiệm vụ đơn giản là nhóm tất thành các nhóm rời rạc có cỡ hai và đếm xem có thể tạo được bao nhiêu nhóm như vậy. 

Đầu vào bao gồm tối đa hai trường hợp thử nghiệm và với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một số nguyên duy nhất$n$, số lượng tất có sẵn. Đầu ra cho mỗi trường hợp thử nghiệm là số lượng đôi tối đa, nghĩa là chúng ta được phép ghép tất một cách tối ưu mà không có ràng buộc bổ sung nào như màu sắc hoặc hạn chế. 

Sự ràng buộc về$n$là cực kỳ nhỏ, từ 34 đến 35. Điều này ngay lập tức loại trừ mọi nhu cầu quan tâm đến tối ưu hóa. Bất kì$O(1)$hoặc thậm chí$O(n)$Cách tiếp cận này là đủ tầm thường. Trong thực tế, cấu trúc của bài toán gợi ý rõ ràng rằng không cần mô phỏng hoặc xử lý phức tạp vì không có cấu trúc bổ sung nào ngoài việc đếm. 

Không có trường hợp phức tạp nào liên quan đến việc đặt hàng hoặc sắp xếp. Trường hợp cạnh tiềm năng duy nhất là liệu$n$là chẵn hoặc lẻ. Nếu như$n$đồng đều, tất cả các loại tất đều có thể được kết hợp hoàn hảo. Nếu như$n$thật kỳ quặc, chính xác là một chiếc tất vẫn chưa được ghép đôi. 

Một sai lầm ngây thơ là cố gắng ghép nối thông qua mô phỏng mà không xử lý phần còn sót lại một cách chính xác. Ví dụ, nếu$n = 35$, một vòng lặp ngây thơ ghép nối hai cái cùng một lúc cho đến khi cạn kiệt có thể vô tình vượt quá hoặc xử lý sai đơn vị cuối cùng nếu được triển khai với giới hạn không chính xác. Câu trả lời đúng trong trường hợp này rõ ràng là$17$, từ$35 = 2 \cdot 17 + 1$. 

Một cách tiếp cận không chính xác khác có thể xảy ra là giả sử một số cấu trúc ẩn trong định dạng đầu vào do mẫu được nối một cách trực quan. Tuy nhiên, mỗi trường hợp kiểm thử là độc lập và chỉ phụ thuộc vào chính nó.$n$. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng quá trình ghép đôi một cách rõ ràng. Chúng ta có thể liên tục tháo hai chiếc tất và đếm xem chúng ta thực hiện bao nhiêu lần tháo như vậy cho đến khi còn lại ít hơn hai chiếc tất. Điều này hoạt động chính xác vì mỗi thao tác tương ứng chính xác với việc tạo thành một cặp hợp lệ. Tuy nhiên, mặc dù điều này đúng nhưng đó là chi phí không cần thiết do tính đơn giản của vấn đề. 

Quan sát quan trọng là việc ghép tất một cách tham lam là tối ưu và mang tính quyết định. Mỗi đôi tiêu thụ chính xác hai chiếc tất, do đó số lượng đôi hoàn toàn được xác định bằng phép chia số nguyên của$n$bằng 2. Không có kịch bản nào trong đó việc lựa chọn khác nhau sẽ thay đổi kết quả, bởi vì không có ràng buộc nào về việc chiếc tất nào có thể kết hợp với chiếc tất nào. 

Điều này làm giảm vấn đề thành một phép toán số học duy nhất cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n)$|$O(1)$| Đã chấp nhận | 
| Công thức trực tiếp |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Mỗi trường hợp thử nghiệm đều độc lập nên chúng tôi xử lý chúng một cách riêng biệt. 
2. Với mỗi test, hãy đọc số nguyên$n$, đại diện cho số lượng tất. 
3. Tính số cặp đầy đủ bằng cách chia$n$cho 2 bằng phép chia số nguyên. Điều này hiệu quả vì mỗi đôi tiêu thụ chính xác hai chiếc tất và bất kỳ chiếc tất nào còn sót lại đều không thể tạo thành một đôi. 
4. Xuất giá trị tính toán cho test case đó. 

### Tại sao nó hoạt động 

Mỗi đôi hợp lệ yêu cầu chính xác hai chiếc tất riêng biệt và không có hạn chế nào ngăn cản việc ghép đôi bất kỳ hai chiếc tất nào. Do đó, vấn đề giảm xuống việc phân vùng một tập hợp kích thước$n$thành các nhóm cỡ 2 càng nhiều càng tốt. Số lượng tối đa của các nhóm rời rạc như vậy chính xác là số sàn của$n/2$. Không có chiến lược ghép nối thay thế nào có thể tăng số lượng này vì mỗi cặp tiêu thụ hai phần tử và không có khả năng tái sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input().strip())
for _ in range(t):
    n = int(input().strip())
    print(n // 2)
```Giải pháp đọc số lượng trường hợp kiểm thử và xử lý từng trường hợp kiểm thử một cách độc lập. Đối với mỗi$n$, phép chia số nguyên cho 2 sẽ trực tiếp cho ra số cặp hoàn chỉnh. 

Chi tiết triển khai duy nhất quan trọng là đảm bảo đầu vào được phân tích cú pháp rõ ràng trên mỗi dòng. Bởi vì$t \le 2$, ngay cả chi phí vô tình cũng không thành vấn đề, nhưng mẫu I/O nhanh tiêu chuẩn vẫn được sử dụng để đảm bảo tính chính xác và nhất quán. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi việc tính toán cho hai đầu vào đại diện. 

### Ví dụ 1 

đầu vào:```
n = 34
```| Bước | n | Tính toán | Cặp | 
| --- | --- | --- | --- | 
| 1 | 34 | 34 // 2 | 17 | 

Điều này cho thấy khi$n$chẵn, tất cả những chiếc tất đều được kết hợp hoàn hảo và không có thức ăn thừa. 

### Ví dụ 2 

đầu vào:```
n = 35
```| Bước | n | Tính toán | Cặp | 
| --- | --- | --- | --- | 
| 1 | 35 | 35 // 2 | 17 | 

Điều này chứng tỏ rằng một chiếc tất còn sót lại không đóng góp vào bất kỳ cặp nào, do đó kết quả giống như đối với 34 ngoại trừ phần tử không được ghép đôi. 

Trong cả hai trường hợp, dấu vết xác nhận rằng thuật toán chỉ phụ thuộc vào tính chẵn lẻ chứ không phụ thuộc vào bất kỳ cấu trúc ẩn nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Một phép chia theo thời gian không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Các ràng buộc đảm bảo tối đa hai trường hợp thử nghiệm và kích thước đầu vào rất nhỏ, do đó giải pháp chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    t = int(input().strip())
    for _ in range(t):
        n = int(input().strip())
        out.append(str(n // 2))
    
    return "\n".join(out)

# provided sample (as interpreted)
assert run("2\n34\n35\n") == "17\n17"

# minimum edge
assert run("1\n34\n") == "17"

# odd boundary
assert run("1\n35\n") == "17"

# small synthetic
assert run("2\n2\n3\n") == "1\n1"

# larger even/odd mix
assert run("2\n100\n101\n") == "50\n50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 34, 35 | 17, 17 | hành vi chẵn lẻ mẫu | 
| 1, 34 | 17 | trường hợp chẵn hợp lệ tối thiểu | 
| 1, 35 | 17 | trường hợp biên lẻ | 
| 2, 2, 3 | 1, 1 | trường hợp ghép đôi nhỏ nhất có ý nghĩa | 
| 2, 100, 101 | 50, 50 | giá trị lớn hơn, tính đúng chẵn lẻ | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là khi$n$thật kỳ quặc. Vì$n = 35$, thuật toán tính toán$35 // 2 = 17$. Trong nội bộ, không cần xử lý đặc biệt vì phép chia số nguyên sẽ loại bỏ phần tất còn sót lại một cách tự nhiên. 

Truy tìm$n = 35$: 

Bước 1 đọc$n = 35$. Bước 2 áp dụng phép chia số nguyên, tạo ra 17. Bước 3 cho kết quả 17. Chiếc tất còn lại hoàn toàn bị bỏ qua vì nó không thể tạo thành một cặp. 

Điều này xác nhận rằng việc triển khai xử lý chính xác cả giá trị chẵn và lẻ mà không cần phân nhánh hoặc logic đặc biệt.
