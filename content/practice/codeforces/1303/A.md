---
title: "CF 1303A - Xóa số 0"
description: "Chúng ta được cung cấp một chuỗi nhị phân, một chuỗi số 0 và 1, và mục tiêu là thao tác nó sao cho tất cả số 1 xuất hiện trong một khối liền kề. Hoạt động duy nhất được phép là loại bỏ một số số 0."
date: "2026-06-11T18:06:01+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1303
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 82 (Rated for Div. 2)"
rating: 800
weight: 1303
solve_time_s: 109
verified: true
draft: false
---

[CF 1303A - Xóa số 0](https://codeforces.com/problemset/problem/1303/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân, một chuỗi số 0 và 1, và mục tiêu là thao tác nó sao cho tất cả số 1 xuất hiện trong một khối liền kề. Hoạt động duy nhất được phép là loại bỏ một số số 0. Chúng ta cần tính số lượng 0 tối thiểu phải được loại bỏ để điều kiện này được giữ nguyên. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một chuỗi có độ dài tối đa là 100. Điều này có nghĩa là chúng ta có thể cung cấp các thuật toán bậc hai theo độ dài chuỗi nếu cần, nhưng các giải pháp thời gian tuyến tính rõ ràng là sạch hơn và đơn giản hơn. Bởi vì chuỗi chỉ có thể chứa 0 và 1 nên chúng ta không phải xử lý bất kỳ ký tự nào khác. 

Trường hợp cạnh không rõ ràng phát sinh khi không có số 1 trong chuỗi, chẳng hạn như "0" hoặc "0000". Trong những trường hợp này, không có khoảng cách giữa các số 1 để loại bỏ, vì vậy câu trả lời là 0. Tương tự, nếu tất cả các ký tự đều bằng 1 thì không cần loại bỏ. Một trường hợp tinh tế khác xảy ra khi có các số 0 ở đầu hoặc cuối, chẳng hạn như "0011100", trong đó cần phải loại bỏ các số 0 ở giữa, nhưng các số 0 ở đầu và cuối không thành vấn đề. Một giải pháp đơn giản có thể đếm không chính xác tất cả các số 0 hoặc không xác định chính xác phân đoạn của số 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên mọi phân đoạn liền kề có thể có của chuỗi và đếm xem có bao nhiêu số 0 bên trong phân đoạn đó. Đối với mỗi phân đoạn bắt đầu và kết thúc bằng 1, chúng tôi sẽ tính số 0 giữa các số 1 này và sau đó lấy giá trị tối thiểu trên tất cả các phân đoạn. Cách tiếp cận này hiệu quả vì nó mô phỏng rõ ràng việc loại bỏ các số 0 để tạo các số 1 liền kề nhau, nhưng nó yêu cầu kiểm tra các phân đoạn O(n^2) và với độ dài chuỗi tối đa là 100, con số này là khoảng 10.000 thao tác cho mỗi trường hợp thử nghiệm. Nó khả thi nhưng phức tạp không cần thiết. 

Quan sát quan trọng dẫn đến giải pháp tối ưu là các số 0 duy nhất quan trọng là các số nằm giữa lần xuất hiện đầu tiên và cuối cùng của số 1. Bất kỳ số 0 nào trước số 1 đầu tiên hoặc sau số 1 cuối cùng đều không thể phá vỡ khối liền kề của số 1 và do đó không cần loại bỏ. Khi chúng ta biết vị trí của số 1 đầu tiên và cuối cùng, chúng ta có thể lặp lại chuỗi con đó và đếm số 0. Số lượng đó chính xác là số 0 tối thiểu để xóa. Quan sát này làm giảm vấn đề xuống còn một lần quét tuyến tính cho mỗi trường hợp thử nghiệm, việc này đơn giản hơn và đủ nhanh với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc chuỗi nhị phân. 
2. Tìm chỉ số xuất hiện đầu tiên của '1'. Nếu không có '1', hãy trả về 0 ngay lập tức vì không cần xóa. 
3. Tìm chỉ số xuất hiện cuối cùng của '1'. 
4. Lặp lại chuỗi con từ '1' đầu tiên đến '1' cuối cùng. Đếm số '0' trong phạm vi này. 
5. Xuất số đếm này. Điều này thể hiện số lượng tối thiểu các số 0 cần xóa để làm cho tất cả các số 1 liền kề nhau. 

Tại sao nó hoạt động: Bất biến là bất kỳ số 0 nào nằm ngoài phạm vi từ đầu đến cuối 1 đều không liên quan. Chỉ đếm số 0 trong phạm vi này đảm bảo rằng sau khi loại bỏ chúng, số 1 sẽ tạo thành một phân đoạn duy nhất không bị gián đoạn. Không có cấu hình nào khác có thể mang lại ít lần xóa hơn vì bất kỳ số 0 nào trong phạm vi đều phải được xóa để hợp nhất các số 1 tách biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    if '1' not in s:
        print(0)
        continue
    first = s.index('1')
    last = s.rindex('1')
    count = s[first:last+1].count('0')
    print(count)
```Giải pháp đầu tiên xử lý trường hợp tầm thường không có số 1. Sau đó, nó xác định chuỗi con chứa tất cả các số 1 và đếm các số 0 bên trong. sử dụng`index`Và`rindex`đảm bảo các giới hạn được xác định chính xác và việc cắt chuỗi từ đầu đến cuối đảm bảo chỉ phần có liên quan được quét. Các lỗi riêng lẻ có thể tránh được bằng cách bao gồm`last+1`trong lát cắt, đây là quy ước Python tiêu chuẩn để bao gồm phần cuối trong phạm vi. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
010011
```| Biến | Giá trị | 
| --- | --- | 
| đầu tiên | 1 | 
| cuối cùng | 5 | 
| s[đầu tiên:cuối+1] | "10011" | 
| số lượng '0' | 2 | 

Thuật toán xác định rằng số 0 ở vị trí 2 và 3 phải được loại bỏ. Đầu ra là 2, phù hợp với kết quả mong đợi. 

### Đầu vào mẫu 2```
0
```| Biến | Giá trị | 
| --- | --- | 
| đầu tiên | không tìm thấy | 
| cuối cùng | không tìm thấy | 
| đầu ra | 0 | 

Không có số 1 nào tồn tại nên không cần phải loại bỏ. Thuật toán trả về đúng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tìm số 1 đầu tiên và cuối cùng, cắt và đếm số 0 đều tuyến tính theo độ dài chuỗi | 
| Không gian | O(1) thêm | Chỉ các chỉ số và bộ đếm được lưu trữ; việc cắt tạo ra một chuỗi con tạm thời nhưng không mở rộng đáng kể theo kích thước đầu vào | 

Cho tối đa`t = 100`và độ dài chuỗi tối đa là 100, thuật toán này thực hiện khoảng 10.000 thao tác, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        s = input().strip()
        if '1' not in s:
            res.append('0')
            continue
        first = s.index('1')
        last = s.rindex('1')
        count = s[first:last+1].count('0')
        res.append(str(count))
    return '\n'.join(res)

# Provided samples
assert run("3\n010011\n0\n1111000\n") == "2\n0\n0", "sample 1"

# Custom test cases
assert run("1\n0000\n") == "0", "all zeros"
assert run("1\n1111\n") == "0", "all ones"
assert run("1\n1010101\n") == "3", "alternating ones and zeros"
assert run("1\n10001\n") == "3", "zeros between first and last ones"
assert run("1\n011110\n") == "0", "zeros outside first and last ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0000 | 0 | tất cả các số 0, không có số 1 để liền kề nhau | 
| 1111 | 0 | tất cả những cái đó, không có số 0 nào để xóa | 
| 1010101 | 3 | mẫu xen kẽ, các số 0 bên trong phải được loại bỏ | 
| 10001 | 3 | số không hoàn toàn giữa số đầu tiên và số cuối 1 | 
| 011110 | 0 | số 0 ở đầu/cuối bị bỏ qua | 

## Vỏ cạnh 

Đối với đầu vào "0000", thuật toán không tìm thấy '1' và trả về 0 ngay lập tức. Đối với "1111", số 1 đầu tiên và cuối cùng nằm ở chỉ số 0 và 3, chuỗi con là chuỗi đầy đủ và không có số 0, trả về 0. Đối với "10001", số 1 đầu tiên và cuối cùng nằm ở chỉ số 0 và 4, chuỗi con là "10001", các số 0 bên trong được tính là 3, phù hợp với trực giác. Tất cả các trường hợp cạnh liên quan đến chuỗi không có số 1, chuỗi chỉ có số 1 và chuỗi có số 0 nằm ngoài khối 1 đều được xử lý chính xác.
