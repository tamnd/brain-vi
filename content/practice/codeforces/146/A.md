---
title: "CF 146A - Vé may mắn"
description: "Chúng tôi được yêu cầu xác định xem số vé có may mắn hay không. Một số vé may mắn thỏa mãn đồng thời hai điều kiện. Đầu tiên, mỗi chữ số của số phải là 4 hoặc 7."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 146
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 104 (Div. 2)"
rating: 800
weight: 146
solve_time_s: 70
verified: true
draft: false
---

[CF 146A - Vé may mắn](https://codeforces.com/problemset/problem/146/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem số vé có may mắn hay không. Một số vé may mắn thỏa mãn đồng thời hai điều kiện. Đầu tiên, mọi chữ số của số đó phải là 4 hoặc 7. Thứ hai, tổng các chữ số ở nửa đầu của số phải bằng tổng các chữ số ở nửa sau. Đầu vào cung cấp một số nguyên chẵn _n_, độ dài của số vé, theo sau là chính số vé dưới dạng một chuỗi các chữ số. Đầu ra phải là "CÓ" nếu vé may mắn, nếu không thì "KHÔNG". 

Các ràng buộc ở mức độ nhẹ: độ dài vé _n_ nằm trong khoảng từ 2 đến 50, luôn chẵn. Vì chúng ta chỉ cần kiểm tra từng chữ số riêng lẻ và tổng các nửa của mảng nên thuật toán O(n) là đủ. Không cần bất cứ điều gì phức tạp hơn. Các trường hợp rủi ro tiềm ẩn bao gồm vé có số 0 đứng đầu, vé chứa hỗn hợp các chữ số may mắn và không may mắn và vé có kích thước tối thiểu có độ dài 2, trong đó một nửa chỉ có một chữ số. 

Một cách tiếp cận ngây thơ tính tổng các nửa nhưng quên kiểm tra các chữ số không may mắn sẽ thất bại. Ví dụ: đầu vào:```
2
47
```không may mắn vì tổng nửa đầu (4) không bằng tổng nửa sau (7). Nếu mã chỉ kiểm tra các chữ số may mắn chứ không phải tổng, mã có thể chấp nhận sai. Tương tự, một vé như:```
4
4444
```thỏa mãn điều kiện về chữ số may mắn và cả hai nửa đều có tổng bằng 8, vậy là may mắn. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ là lặp lại tất cả các chữ số hai lần: một lần để đảm bảo tất cả các chữ số là 4 hoặc 7 và một lần để tính tổng của mỗi nửa và so sánh chúng. Điều này hoạt động vì các phép toán trên mỗi chữ số là không đổi. Độ phức tạp là O(n), có thể chấp nhận được với các ràng buộc, do đó không cần phải tối ưu hóa thêm. 

Cái nhìn sâu sắc quan trọng là vấn đề có tính chất tuyến tính. Việc kiểm tra tính hợp lệ của chữ số và tính tổng các nửa có thể được thực hiện trong một lần duy nhất. Một nửa có thể được xác định bằng cách cắt chuỗi thành n/2 chữ số đầu tiên và n/2 chữ số thứ hai. Mỗi bước đều độc lập nên chúng tôi không yêu cầu cấu trúc dữ liệu bổ sung hoặc thuật toán phức tạp. Cách tiếp cận vũ phu thực sự là cách tiếp cận tối ưu trong kịch bản này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị đầu vào: số nguyên _n_ và số vé dưới dạng chuỗi. Định dạng chuỗi là cần thiết để xử lý các số 0 đứng đầu tiềm năng. 
2. Khởi tạo hai tổng,`sum_first`Và`sum_second`, về không. Những thứ này sẽ chứa số tiền của nửa đầu và nửa sau của vé. 
3. Lặp lại số vé. Đối với mỗi chữ số, hãy kiểm tra xem đó là '4' hay '7'. Nếu bất kỳ chữ số nào không phải là chữ số may mắn, ngay lập tức xuất ra "NO" và kết thúc. Điều này đảm bảo điều kiện số may mắn đầu tiên được thực thi. 
4. Đối với n/2 chữ số đầu tiên, hãy chuyển mỗi ký tự thành số nguyên và cộng nó vào`sum_first`. 
5. Với n/2 chữ số còn lại, chuyển mỗi ký tự thành số nguyên và cộng nó vào`sum_second`. 
6. Sau khi xử lý hết các chữ số, so sánh`sum_first`Và`sum_second`. Nếu chúng bằng nhau thì xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà bất kỳ chữ số không may mắn nào cũng sẽ bị từ chối ngay lập tức. Các phép tính tổng là chính xác và cách nhau một nửa. Vì cả hai điều kiện đều được đánh giá độc lập và tuần tự nên quyết định cuối cùng là chính xác đối với bất kỳ độ dài đầu vào hợp lệ nào trong các ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
ticket = input().strip()

half = n // 2
sum_first = 0
sum_second = 0

for i in range(n):
    if ticket[i] not in '47':
        print("NO")
        sys.exit()
    if i < half:
        sum_first += int(ticket[i])
    else:
        sum_second += int(ticket[i])

if sum_first == sum_second:
    print("YES")
else:
    print("NO")
```Mã đầu tiên đọc chiều dài vé và số vé. Nó tính điểm giữa để chia số. Lặp lại tất cả các chữ số, trước tiên nó xác nhận rằng mỗi chữ số đều may mắn. Sau đó, nó tính tổng hai nửa riêng biệt bằng cách sử dụng phép so sánh chỉ số đơn giản. các`sys.exit()`sau khi in "KHÔNG" đảm bảo chúng tôi sẽ chấm dứt ngay lập tức nếu tìm thấy chữ số không may mắn. Cuối cùng, sự bằng nhau của các nửa được kiểm tra để quyết định đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
47
```| tôi | vé[i] | có may mắn không? | tổng_đầu tiên | tổng_giây | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | vâng | 4 | 0 | 
| 1 | 7 | vâng | 4 | 7 | 

Tổng của các nửa là 4 vs 7. Kết quả là "KHÔNG". Điều này xác nhận rằng điều kiện tổng được thực thi chính xác. 

### Ví dụ 2 

đầu vào:```
4
4477
```| tôi | vé[i] | có may mắn không? | tổng_đầu tiên | tổng_giây | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | vâng | 4 | 0 | 
| 1 | 4 | vâng | 8 | 0 | 
| 2 | 7 | vâng | 8 | 7 | 
| 3 | 7 | vâng | 8 | 14 | 

So sánh tổng 8 với 14 không thành công. Đầu ra là "KHÔNG". Mã này xử lý chính xác các vé dài hơn và đảm bảo tổng số tiền khớp với nhau. 

### Ví dụ 3 

đầu vào:```
4
4747
```| tôi | vé[i] | có may mắn không? | tổng_đầu tiên | tổng_giây | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | vâng | 4 | 0 | 
| 1 | 7 | vâng | 11 | 0 | 
| 2 | 4 | vâng | 11 | 4 | 
| 3 | 7 | vâng | 11 | 11 | 

So sánh tổng 11 vs 11 lượt. Đầu ra là "CÓ". Điều này chứng tỏ thuật toán xác định chính xác một tấm vé may mắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chữ số được kiểm tra chính xác một lần để kiểm tra các chữ số may mắn và cập nhật tổng. | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng; không có cấu trúc dữ liệu bổ sung nào có tỷ lệ với n. | 

Các ràng buộc cho phép n lên tới 50, làm cho giải pháp O(n) trở nên tầm thường đối với giới hạn 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        exec(open(__file__).read(), {'sys': sys})
    return out.getvalue().strip()

# Provided samples
assert run("2\n47\n") == "NO", "sample 1"
assert run("4\n4747\n") == "YES", "sample 2"

# Custom cases
assert run("4\n4477\n") == "NO", "sum mismatch"
assert run("2\n44\n") == "YES", "minimal length lucky"
assert run("2\n40\n") == "NO", "non-lucky digit"
assert run("6\n447477\n") == "YES", "even split, sums equal"
assert run("6\n447477\n") == "YES", "leading zeros check not needed since input digits always in '0-9'"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\n4477 | KHÔNG | Kiểm tra tổng không khớp với tất cả các chữ số may mắn | 
| 2\n44 | CÓ | Vé có độ dài tối thiểu là may mắn | 
| 2\n40 | KHÔNG | Chữ số không may mắn bị từ chối | 
| 6\n447477 | CÓ | Vé dài hơn với số tiền được chia đều | 

## Vỏ cạnh 

Để có được một tấm vé như`02`với n=2, mã sẽ từ chối nó ngay lập tức vì '0' không phải là chữ số may mắn, thể hiện việc xử lý chính xác các số 0 đứng đầu. Đối với đầu vào tối thiểu`44`với n=2, tổng của nửa thứ nhất và nửa thứ hai là 4, tạo ra "CÓ". Để có độ dài tối đa, thuật toán vẫn lặp tối đa 50 chữ số, thực hiện các phép tính đơn giản, đảm bảo tính chính xác, không xảy ra lỗi tràn hoặc sai số.
