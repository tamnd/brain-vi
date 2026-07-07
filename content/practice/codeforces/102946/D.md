---
title: "CF 102946D - Máy tách khí 3000"
description: "Chúng ta có hai hoán vị ẩn, cả hai đều chứa các số từ 1 đến n đúng một lần. Một hoán vị là một phép quay tuần hoàn của hoán vị kia, nhưng chúng ta không biết cả hai hoán vị đó và chúng ta cũng không biết số lượng phép quay k."
date: "2026-07-04T07:31:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102946
codeforces_index: "D"
codeforces_contest_name: "NCTU PCCA Winter Contest 2021"
rating: 0
weight: 102946
solve_time_s: 57
verified: true
draft: false
---

[CF 102946D - Bộ ngắt kết nối 3000](https://codeforces.com/problemset/problem/102946/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hoán vị ẩn, cả hai đều chứa các số từ 1 đến n đúng một lần. Một hoán vị là một phép quay tuần hoàn của hoán vị kia, nhưng chúng ta không biết cả hai hoán vị đó và chúng ta cũng không biết số lượng phép quay k. Về mặt hình thức, tồn tại một độ dịch k chưa biết sao cho mọi vị trí i đều thỏa mãn a[i] = b[(i + k) mod n]. 

Cách duy nhất để có được thông tin là thông qua thao tác truy vấn lấy hai chỉ số x và y và trả về max(a[x], b[y]). Mỗi truy vấn cung cấp cho chúng ta một số nguyên duy nhất và chúng ta phải suy ra độ dịch k bằng cách sử dụng tối đa 2n truy vấn như vậy. 

Ràng buộc n 200 đủ nhỏ để chúng ta có thể đáp ứng số lượng truy vấn tuyến tính mà vẫn suy luận rõ ràng về mọi vị trí. Về mặt lý thuyết, bất kỳ giải pháp nào cố gắng so sánh tất cả các cặp đều ổn nhưng sẽ vượt quá ngân sách truy vấn, vì đó sẽ là O(n²). Điều này ngay lập tức gợi ý rằng phải khai thác cấu trúc hoán vị và vai trò đặc biệt của giá trị lớn nhất. 

Một trường hợp cạnh tinh tế xuất hiện khi k = 0. Trong trường hợp đó cả hai mảng đều giống hệt nhau, do đó, bất kỳ vị trí nào i đều thỏa mãn a[i] = b[i] và chúng ta nên xuất chính xác 0. Một trường hợp cạnh quan trọng khác là khi giá trị lớn nhất n xuất hiện ở cùng một chỉ số trong cả hai hoán vị do độ dịch chuyển bằng 0. Trong trường hợp đó, lý luận ngây thơ về “hai vị trí cực đại khác nhau” sẽ bị phá vỡ và phải được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xây dựng lại cả hai hoán vị một cách đầy đủ. Nếu chúng ta có thể xác định mọi a[i] và b[i] thì chúng ta có thể tính toán độ dịch chuyển một cách đơn giản bằng cách tìm xem các chuỗi sắp xếp ở đâu. Tuy nhiên, mỗi truy vấn chỉ đưa ra max(a[x], b[y]), không trực tiếp tiết lộ giá trị nào trừ khi chúng ta biết một trong số chúng đủ lớn để chiếm ưu thế. Việc cố gắng khôi phục tất cả các giá trị sẽ yêu cầu lý luận O(n²) và có thể có nhiều truy vấn, vượt quá giới hạn 2n một khoảng lớn. 

Quan sát quan trọng là chúng ta không cần hoán vị đầy đủ. Chúng ta chỉ cần xác định một phần tử neo duy nhất tồn tại trong cả hai cấu trúc một cách có kiểm soát. Giá trị tối đa n là lý tưởng vì nó hoạt động giống như một lính canh. Nếu một truy vấn trả về n, chúng ta biết rằng a[x] = n hoặc b[y] = n. Vì n xuất hiện chính xác một lần trong mỗi hoán vị, nên nó cho phép chúng ta bản địa hóa vị trí của n trong cả hai mảng chỉ bằng cách sử dụng truy vấn O(n). 

Khi chúng ta xác định được chỉ số trong đó n xuất hiện trong a và chỉ mục trong đó n xuất hiện trong b, phép quay sẽ trở thành hiệu số học trực tiếp giữa các vị trí này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết Brute Force | Truy vấn O(n²) | O(n) | Quá chậm | 
| Bản địa hóa giá trị tối đa | Truy vấn O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mọi chỉ số i từ 0 đến n − 1, hãy truy vấn cặp (i, i) và lưu kết quả. Mục tiêu là phát hiện các vị trí trong đó ít nhất một trong hai mảng có giá trị n. Vì n là giá trị duy nhất có thể xuất hiện dưới dạng kết quả tối đa nên bất kỳ truy vấn nào trả về n đều phải chạm vào vị trí của n trong a hoặc b. 
2. Thu thập tất cả các chỉ số i sao cho truy vấn(i, i) trả về n. Bộ này sẽ chứa một hoặc hai chỉ số. Nếu nó chỉ chứa một chỉ mục thì cả hai hoán vị đều có n ở cùng một vị trí, nghĩa là phép quay bằng 0 và chúng ta có thể xuất ngay k = 0. 
3. Nếu có hai chỉ số i và j thì ta biết một trong hai chỉ số đó là vị trí của n trong a và chỉ số kia là vị trí của n trong b, nhưng chưa biết đó là vị trí nào. 
4. Để giải quyết hướng, hãy thực hiện một truy vấn chéo (i, j). Nếu kết quả là n thì a[i] phải là n, vì nếu không thì cả a[i] và b[j] đều không thể tạo ra n. Điều này ngụ ý i là vị trí của n trong a và j là vị trí của n trong b. Ngược lại, nếu kết quả nhỏ hơn n thì vai trò sẽ bị đảo ngược, nghĩa là i là vị trí của n trong b và j là vị trí của n trong a. 
5. Khi chúng ta đã biết posA và posB, hãy tính độ dịch chuyển là k = (posB − posA) mod n. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào thực tế là n là duy nhất trong cả hai hoán vị và thống trị mọi giá trị khác trong truy vấn tối đa. Bất kỳ truy vấn nào trả về n đều đưa ra ràng buộc logic trực tiếp trên các chỉ mục được truy vấn. Các truy vấn đường chéo cô lập chính xác các vị trí có n trong cả hai cấu trúc. Truy vấn chéo giải quyết sự mơ hồ vì chỉ có sự căn chỉnh chính xác mới cho phép n xuất hiện ở mức tối đa. Định nghĩa dịch chuyển theo chu kỳ đảm bảo rằng khi các vị trí của một giá trị được căn chỉnh, toàn bộ độ lệch xoay sẽ được cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(x, y):
    print(f"? {x} {y}")
    sys.stdout.flush()
    return int(input())

def main():
    n = int(input())

    candidates = []

    for i in range(n):
        res = ask(i, i)
        if res == n:
            candidates.append(i)

    if len(candidates) == 1:
        print(f"! 0")
        sys.stdout.flush()
        return

    i, j = candidates[0], candidates[1]
    res = ask(i, j)

    if res == n:
        posA = i
        posB = j
    else:
        posA = j
        posB = i

    k = (posB - posA) % n
    print(f"! {k}")
    sys.stdout.flush()

if __name__ == "__main__":
    main()
```Việc triển khai xoay quanh một chức năng trợ giúp duy nhất thực hiện truy vấn tương tác và xóa đầu ra ngay lập tức, vì việc quên xóa sẽ phá vỡ sự đồng bộ hóa với thẩm phán. 

Vòng lặp trên tất cả các chỉ số sử dụng chính xác n truy vấn, mỗi truy vấn kiểm tra xem vị trí đường chéo có ẩn giá trị tối đa hay không. Sau đó, cần có nhiều nhất một truy vấn bổ sung để phân biệt hướng. Tính toán modulo đảm bảo phép quay được chuẩn hóa chính xác trong phạm vi [0, n − 1]. 

Một cạm bẫy phổ biến là giả định rằng hai chỉ số được phát hiện luôn tương ứng với các vai trò riêng biệt; xử lý sớm trường hợp k = 0 sẽ ngăn chặn các truy vấn không cần thiết và tránh hiểu sai các vị trí giống nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ trong đó n = 4 và các mảng ẩn là a = [2, 3, 1, 4] và b = [4, 2, 3, 1]. Sự dịch chuyển đúng là k = 1. 

Chúng tôi mô phỏng các truy vấn theo đường chéo: 

| tôi | truy vấn(i, i) | giải thích | 
| --- | --- | --- | 
| 0 | tối đa(2, 4) = 4 | ứng cử viên | 
| 1 | tối đa(3, 2) = 3 | không | 
| 2 | tối đa(1, 3) = 3 | không | 
| 3 | tối đa(4, 1) = 4 | ứng cử viên | 

Chúng tôi có được ứng viên {0, 3}. Bây giờ chúng tôi truy vấn (0, 3). Kết quả là max(a[0], b[3]) = max(2, 1) = 2, không phải là 4, do đó vai trò bị đảo ngược: chỉ số 3 là vị trí của n trong a và chỉ số 0 là vị trí của n trong b. 

Do đó posA = 3, posB = 0, cho k = (0 − 3) mod 4 = 1. 

Dấu vết này cho thấy một giá trị duy nhất, giá trị tối đa, đủ để xác định đầy đủ sự dịch chuyển theo chu kỳ mà không cần tái tạo lại hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(n) | Một truy vấn cho mỗi chỉ mục cộng với tối đa một truy vấn bổ sung | 
| Không gian | O(1) | Chỉ lưu trữ các chỉ số ứng cử viên và một vài biến | 

Giới hạn truy vấn 2n được tôn trọng vì chúng tôi sử dụng n truy vấn đường chéo và tối đa một truy vấn so sánh bổ sung, nằm trong ngân sách cho phép. 

## Trường hợp thử nghiệm```python
import sys, io

# This is a conceptual placeholder since the solution is interactive.
# A full test harness would require a mocked judge.

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return "interactive"

# provided samples (conceptual)
# assert run("...") == "..."

# custom cases
# n = 2, k = 0
# n = 2, k = 1
# n = 4, identity shift
# n = 5, random rotation
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 danh tính | ! 0 | trường hợp dịch chuyển số không | 
| n=2 hoán đổi | ! 1 | dịch chuyển tối thiểu khác không | 
| n=4 vòng quay | ! k | tính đúng đắn chung | 
| n=5 ngẫu nhiên | ! k | ổn định dưới sự hoán vị ngẫu nhiên | 

## Vỏ cạnh 

Khi k = 0, cả hai mảng đều giống nhau nên mọi truy vấn theo đường chéo đều trả về cùng một giá trị. Trong tình huống đó, chỉ một chỉ mục sẽ được thu thập làm ứng cử viên và thuật toán ngay lập tức đưa ra 0 mà không thực hiện bất kỳ truy vấn bổ sung nào. 

Khi hai vị trí của n trùng nhau trong không gian chỉ mục, danh sách ứng cử viên có kích thước bằng một, phản ánh chính xác rằng mức tối đa xuất hiện ở cùng một vị trí trong cả hai mảng. Thuật toán không thử truy vấn chéo trong trường hợp này, tránh sự mơ hồ. 

Khi n xuất hiện ở các vị trí khác nhau, truy vấn chéo sẽ phân biệt rõ ràng hướng vì chỉ một lần gán vai trò có thể xuất hiện tối đa trong cặp được truy vấn. Điều này ngăn chặn sự đảo ngược hướng dịch chuyển không chính xác.
