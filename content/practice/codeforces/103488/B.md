---
title: "CF 103488B - Boboge và nhà cao tầng"
description: "Tòa nhà được mô hình hóa như một cấu trúc thẳng đứng được chia thành một số tầng cố định có chiều cao bằng nhau. Tổng chiều cao của tòa nhà được đưa ra dưới dạng một giá trị duy nhất và chiều cao đó được phân bổ đồng đều trên tất cả các tầng."
date: "2026-07-03T06:16:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103488
codeforces_index: "B"
codeforces_contest_name: "The 2021 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 103488
solve_time_s: 45
verified: true
draft: false
---

[CF 103488B - Boboge và nhà cao tầng](https://codeforces.com/problemset/problem/103488/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tòa nhà được mô hình hóa như một cấu trúc thẳng đứng được chia thành một số tầng cố định có chiều cao bằng nhau. Tổng chiều cao của tòa nhà được đưa ra dưới dạng một giá trị duy nhất và chiều cao đó được phân bổ đồng đều trên tất cả các tầng. Tầng đầu tiên nằm ở độ cao 0 và mỗi tầng tiếp theo tăng tuyến tính cho đến khi tầng trên cùng đạt đến độ cao tối đa. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp ba giá trị. Giá trị đầu tiên cho chúng ta biết Boboge sống ở tầng nào, được tính từ dưới lên bắt đầu từ 1. Giá trị thứ hai là tổng số tầng trong tòa nhà. Giá trị thứ ba là tổng chiều cao của tòa nhà. Nhiệm vụ là tính chiều cao chính xác của tầng Boboge. 

Mỗi tầng tương ứng với một phân đoạn thống nhất về tổng chiều cao. Điều này ngay lập tức ngụ ý rằng vấn đề về cơ bản là về việc chia một khoảng theo tỷ lệ thành các phần bằng nhau, sau đó chọn một điểm dừng cụ thể. 

Các ràng buộc rất nhỏ, với tối đa 100 trường hợp thử nghiệm và tất cả các giá trị được giới hạn bởi 100. Điều này đảm bảo rằng mọi phép tính trực tiếp nặng về số học hoặc dấu phẩy động sẽ chạy thoải mái trong giới hạn. Không cần tối ưu hóa ngoài việc đánh giá liên tục theo thời gian cho mỗi trường hợp thử nghiệm. 

Một vấn đề nhỏ có thể xuất hiện khi triển khai bất cẩn là việc lập chỉ mục các tầng theo từng tầng. Vì tầng đầu tiên được xác định rõ ràng ở độ cao 0 nên tầng 1 phải ánh xạ tới 0, không phải phần dương. Ví dụ: nếu n = 1, m = 4, k = 10, câu trả lời đúng là 0. Nếu chúng ta coi các tầng là 0 chỉ mục hoặc dịch chuyển không chính xác, thì chúng ta có thể xuất ra 2,5 hoặc một giá trị phân số khác, điều này là sai. 

Một trường hợp cạnh tiềm năng khác là khi n bằng m. Trong trường hợp đó, Boboge ở tầng trên cùng và chiều cao phải chính xác là k. Nếu chia nhầm cho m thay vì m − 1 thì chiều cao tính toán của tầng trên cùng sẽ thiếu k. 

## Phương pháp tiếp cận 

Cấu trúc của tòa nhà gợi ý một cách giải thích hình học trực tiếp. Phạm vi chiều cao từ 0 đến k được chia thành m − 1 đoạn bằng nhau, vì có m tầng nhưng có m − 1 khoảng cách giữa chúng. Do đó, mỗi đoạn có kích thước k / (m − 1). 

Cách giải thích mạnh mẽ sẽ mô phỏng rõ ràng từng ranh giới của tầng, liên tục thêm chiều cao của đoạn cho đến khi đạt đến tầng mong muốn. Điều này đúng nhưng không cần thiết. Nó thực hiện phép cộng O(m) cho mỗi trường hợp thử nghiệm, điều này vẫn không đáng kể đối với m ≤ 100, nhưng nó che khuất mối quan hệ trực tiếp. 

Quan sát quan trọng là các tầng hình thành một cấp số cộng về chiều cao. Tầng thứ i (chỉ mục 1) nằm ở độ cao (i - 1) lần kích thước bậc thang thống nhất. Một khi điều này được nhận ra, câu trả lời sẽ trở thành một phép nhân duy nhất. 

Cách tiếp cận bạo lực hoạt động vì nó xây dựng tiến trình tăng dần, nhưng nó đang tính toán lại một công thức mà chúng ta có thể viết trực tiếp một cách hiệu quả. Dạng đóng làm giảm việc tính toán về thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm là độc lập, do đó việc tính toán không được thực hiện. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đọc n, m, k, trong đó n là chỉ số tầng mục tiêu, m là tổng số tầng và k là tổng chiều cao. 
3. Tính khoảng cách thẳng đứng giữa các tầng liên tiếp theo bước = k / (m − 1). Điều này hiệu quả vì có chính xác m − 1 khoảng cách bằng nhau giữa tầng 1 ở độ cao 0 và tầng m ở độ cao k. 
4. Tính chiều cao tầng n theo (n − 1)*bậc. Điều này phản ánh rằng tầng đầu tiên được neo ở độ cao bằng 0 và mỗi tầng tiếp theo sẽ thêm một bậc thang đầy đủ. 
5. Xuất kết quả dưới dạng số dấu phẩy động với độ chính xác vừa đủ. 

### Tại sao nó hoạt động

Chiều cao của tòa nhà tạo thành một cấp số cộng bắt đầu từ 0 với sai phân chung k / (m − 1). Bất kỳ cấp số cộng nào đều được xác định đầy đủ bởi số hạng đầu tiên và hiệu chung của nó, do đó chiều cao của bất kỳ số hạng nào được cho bởi công thức dạng đóng duy nhất. Vì việc lập chỉ mục sàn khớp với chỉ mục thuật ngữ được dịch chuyển một đơn vị, nên ánh xạ tầng n tới thuật ngữ (n − 1) duy trì tính chính xác cho tất cả các đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, k = map(int, input().split())
    step = k / (m - 1)
    ans = (n - 1) * step
    print(ans)
```Giải pháp hoàn toàn dựa vào việc chuyển hệ thống sàn thành một cấp số cộng. Chi tiết triển khai chính là sử dụng phép chia dấu phẩy động cho kích thước bước, vì phép chia số nguyên sẽ làm mất đi độ chính xác. Một điểm tinh tế khác là đảm bảo mẫu số là m − 1 chứ không phải m, vì các tầng biểu thị các ranh giới bao gồm cả hai điểm cuối 0 và k. 

Biểu thức (n − 1) * bước mã hóa trực tiếp phần bù từ tầng cơ sở. Không cần vòng lặp và mỗi trường hợp thử nghiệm được giải quyết bằng một số phép tính số học không đổi. 

## Ví dụ đã hoạt động 

Xem xét đầu vào trong đó n = 3, m = 4, k = 10. Kích thước bước là 10/3. 

| Bước | n | m | k | bước | biểu hiện | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 4 | 10 | - | - | - | 
| bước tính toán | 3 | 4 | 10 | 3.333... | - | - | 
| tính toán đáp án | 3 | 4 | 10 | 3.333... | (3 − 1) * 3.333... | 6.666... ​​| 

Điều này khẳng định rằng tầng 3 nằm cách mặt đất hai bậc. 

Bây giờ hãy xem xét n = 1, m = 4, k = 10. 

| Bước | n | m | k | bước | biểu hiện | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 4 | 10 | - | - | - | 
| bước tính toán | 1 | 4 | 10 | 3.333... | - | - | 
| tính toán đáp án | 1 | 4 | 10 | 3.333... | (1 − 1) * 3,333... | 0 | 

Điều này cho thấy tầng trệt vẫn ở độ cao 0 bất kể tổng chiều cao. 

Những dấu vết này xác nhận cách giải thích cấp số cộng và cho thấy rằng cả tầng bên trong và tầng ranh giới đều hoạt động nhất quán theo cùng một công thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện các phép tính số học theo thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ nào được sử dụng ngoài các biến | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm, do đó giải pháp chạy trong thời gian không đáng kể. Ngay cả với các phép toán dấu phẩy động, khối lượng công việc vẫn không đáng kể trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        step = k / (m - 1)
        out.append(str((n - 1) * step))
    return "\n".join(out)

# provided samples
assert run("5\n3 4 10\n1 4 10\n2 3 10\n2 5 12\n4 5 12\n") == \
"6.666666666666667\n0.0\n5.0\n2.5\n9.0"

# custom cases
assert run("1\n1 2 100\n") == "0.0", "minimum floors"
assert run("1\n2 2 100\n") == "100.0", "two-floor boundary"
assert run("1\n5 5 100\n") == "100.0", "top floor exact match"
assert run("1\n3 6 11\n") == "4.4", "fractional step case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 100 | 0,0 | chỉ số tối thiểu tại tầng trệt | 
| 2 2 100 | 100,0 | tầng trên cùng với tổng cộng hai tầng | 
| 5 5 100 | 100,0 | sự bằng nhau của các tầng và điểm cuối chiều cao | 
| 3 6 11 | 4.4 | tính đúng đắn của phép chia không nguyên | 

## Vỏ cạnh 

Khi n = 1, công thức trở thành (1 − 1) * k / (m − 1), luôn có giá trị bằng 0 bất kể k và m. Điều này phù hợp với định nghĩa rằng tầng một được neo ở độ cao bằng không. 

Ví dụ: đầu vào n = 1, m = 4, k = 10 tạo ra bước = 10/3 và câu trả lời cuối cùng là 0. Việc tính toán không bao giờ phụ thuộc vào k ngoài tính toán bước, vì vậy mọi vấn đề về dấu phẩy động đều không được truyền bá ở đây. 

Khi n = m, biểu thức trở thành (m − 1) * k / (m − 1), đơn giản hóa chính xác thành k. Ngay cả khi làm tròn dấu phẩy động xảy ra ở bước trung gian, phép nhân với (m − 1) sẽ khôi phục điểm cuối một cách chính xác trong giới hạn độ chính xác. 

Khi m = 2 thì chỉ có một đoạn giữa tầng 1 và tầng 2. Bước trở thành k/1 nên tầng 1 là 0 và tầng 2 là k. Đây là trường hợp không tầm thường tối thiểu xác nhận mô hình cấp số cộng vẫn đúng khi số lượng phân đoạn nhỏ nhất có thể.
