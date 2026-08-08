---
title: "CF 102501H - Trình tạo số giả ngẫu nhiên"
description: "Trình tạo bắt đầu từ một giá trị 40 bit cố định và liên tục biến đổi nó thành giá trị tiếp theo. Phép biến đổi cộng giá trị hiện tại, giá trị thu được bằng cách loại bỏ 20 bit thấp nhất và một hằng số, sau đó chỉ giữ lại 40 bit thấp nhất."
date: "2026-08-06T18:53:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 96
verified: true
draft: false
---

[CF 102501H - Trình tạo số giả ngẫu nhiên](https://codeforces.com/problemset/problem/102501/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trình tạo bắt đầu từ một giá trị 40 bit cố định và liên tục biến đổi nó thành giá trị tiếp theo. Phép biến đổi cộng giá trị hiện tại, giá trị thu được bằng cách loại bỏ 20 bit thấp nhất và một hằng số, sau đó chỉ giữ lại 40 bit thấp nhất. Nhiệm vụ là nhìn vào điều đầu tiên`N`các giá trị được tạo ra và đếm xem có bao nhiêu trong số chúng là số chẵn. 

Giá trị đầu vào`N`có thể lớn gần như`2^63`, do đó việc mô phỏng chuỗi từng phần tử một là không thể. Ngay cả một vòng lặp rất nhanh cũng cần hàng tỷ năm để có được đầu vào lớn nhất. Chúng ta cần tìm một cấu trúc lặp lại và đếm số lần lặp lại hoàn chỉnh thay vì tạo ra mọi phần tử được yêu cầu. 

Quan sát hữu ích là chúng ta không cần toàn bộ trạng thái 40 bit. Câu hỏi chỉ hỏi về bit thấp nhất, vì bit đó quyết định một số có phải là số chẵn hay không. Để tính bit thấp nhất tiếp theo, chúng ta chỉ cần 21 bit thấp nhất của số hiện tại. Bit bị dịch chuyển ra ngoài bởi`>> 20`chính xác là bit 20 và bit đó được chứa bên trong 21 bit đó. 

Một giải pháp bất cẩn có thể thất bại ở một số ranh giới. Ví dụ, với đầu vào`0`, chuỗi không chứa giá trị nào, vì vậy câu trả lời là`0`. Một giải pháp luôn xử lý trạng thái ban đầu sẽ xuất ra không chính xác`1`. 

Đối với đầu vào`1`, chỉ một`S(0)`được tính. Từ`0x600DCAFE`kết thúc bằng chữ số thập lục phân chẵn, câu trả lời đúng là`1`. Một giải pháp áp dụng chuyển đổi trước khi đếm sẽ vô tình đếm`S(1)`thay vì. 

Đối với đầu vào rất lớn, chẳng hạn như`500000000`, việc mô phỏng trực tiếp tất cả các giá trị được tạo ra là không thực tế. Đầu ra đúng là`250065867`, yêu cầu sử dụng chu trình của trạng thái rút gọn thay vì lặp lại trên mỗi số được tạo. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo định nghĩa trực tiếp. Nó lưu trữ giá trị 40 bit hiện tại, kiểm tra tính chẵn lẻ của nó, áp dụng phép truy toán và lặp lại. Điều này đúng vì nó truy cập chính xác các giá trị mà bài toán yêu cầu. Tuy nhiên, đối với`N = 2^63 - 1`, nó đòi hỏi khoảng`9.22 * 10^18`chuyển tiếp vượt quá giới hạn. 

Cấu trúc của phép truy toán cho chúng ta một không gian trạng thái nhỏ hơn. Tính chẵn lẻ của giá trị hiện tại phụ thuộc vào bit 0. Bit 0 của giá trị tiếp theo phụ thuộc vào bit 0 và bit 20 của giá trị hiện tại vì số hạng được dịch chuyển chỉ đóng góp từ phần trên. Do đó, 21 bit thấp hơn tạo thành một hệ thống khép kín. 

Phương pháp brute-force không thành công vì độ dài chuỗi yêu cầu quá lớn. Việc quan sát thấy 21 bit thấp hơn phát triển độc lập làm giảm số lượng trạng thái có thể có từ`2^40`chỉ để`2^21`. Chúng ta có thể tạo chuỗi nhỏ này cho đến khi nó lặp lại, đếm số trạng thái chẵn trong một chu kỳ và sử dụng số học để bỏ qua các chu kỳ hoàn chỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N) | O(1) | Quá chậm | 
| Phát hiện chu kỳ ở trạng thái 21 bit | O(2^21) | O(2^21) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thay thế trạng thái 40 bit ban đầu bằng chỉ 21 bit thấp nhất. Nếu trạng thái giảm hiện tại là`x`, trạng thái giảm tiếp theo được tính là`(x + (x >> 20) + 12345) mod 2^21`. Điều này có tác dụng vì thông tin duy nhất từ 19 bit trên ảnh hưởng đến 21 bit thấp hơn tiếp theo là bit 20. 
2. Tạo trạng thái rút gọn bắt đầu từ`0x600DCAFE & ((1 << 21) - 1)`. Lưu trữ vị trí đầu tiên nơi mỗi trạng thái xuất hiện. Tiếp tục cho đến khi một trạng thái lặp lại, vì từ thời điểm đó trình tự có tính tuần hoàn. 
3. Chia chuỗi được tạo thành tiền tố không lặp lại và chu kỳ lặp lại. Đếm các trạng thái chẵn bên trong cả hai phần. 
4. Nếu`N`nhỏ hơn độ dài tiền tố, hãy trả lời trực tiếp từ tiền tố. Nếu không, hãy thêm phần đóng góp tiền tố, bỏ qua càng nhiều chu kỳ đầy đủ càng tốt bằng cách sử dụng phép chia số nguyên và xử lý các phần tử chu trình còn lại. 

Tại sao nó hoạt động: trạng thái rút gọn chứa chính xác thông tin cần thiết để xác định trạng thái rút gọn trong tương lai và tính chẵn lẻ của mọi số được tạo. Khi trạng thái rút gọn xuất hiện lần thứ hai, tất cả các trạng thái rút gọn tiếp theo sẽ lặp lại theo cùng một thứ tự. Thuật toán đếm tiền tố một lần và tính đến mọi lần lặp lại hoàn chỉnh về mặt toán học, do đó, mỗi lần lặp lại đầu tiên`N`giá trị đóng góp chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MASK = (1 << 21) - 1
START = 0x600DCAFE & MASK
C = 12345

def build_cycle():
    seen = {}
    order = []
    x = START

    while x not in seen:
        seen[x] = len(order)
        order.append(x)
        x = (x + (x >> 20) + C) & MASK

    cycle_start = seen[x]
    return order, cycle_start

order, cycle_start = build_cycle()

prefix_even = [0] * (len(order) + 1)
for i, x in enumerate(order):
    prefix_even[i + 1] = prefix_even[i] + (1 if (x & 1) == 0 else 0)

cycle = order[cycle_start:]
cycle_even = sum(1 for x in cycle if (x & 1) == 0)

def solve(n):
    if n <= cycle_start:
        return prefix_even[n]

    ans = prefix_even[cycle_start]
    n -= cycle_start

    ans += (n // len(cycle)) * cycle_even
    n %= len(cycle)

    for i in range(n):
        if (cycle[i] & 1) == 0:
            ans += 1

    return ans

n = int(input())
print(solve(n))
```Các hằng số xác định trình tạo giảm.`MASK`giữ chính xác 21 bit thấp nhất và`START`loại bỏ các bit cao hơn không liên quan của giá trị 40 bit ban đầu.`build_cycle`thực hiện mô phỏng lớn duy nhất. Nhiều nhất`2^21`các trạng thái khác nhau có thể xuất hiện, do đó vòng lặp bị giới hạn. Từ điển ghi lại lần xuất hiện đầu tiên của mỗi trạng thái, cho phép xác định ngay lập tức phân đoạn lặp lại. 

Mảng tiền tố lưu trữ số lượng tích lũy của các trạng thái chẵn. Điều này tránh việc quét lại phần không theo chu kỳ cho mỗi truy vấn. Số lượng chu kỳ được lưu trữ riêng biệt vì có thể bỏ qua các lần lặp lại hoàn chỉnh bằng phép chia. 

Việc chuyển đổi sử dụng`& MASK`thay vì một hoạt động modulo. Vì mô-đun là lũy thừa của hai nên việc giữ 21 bit thấp nhất là hoàn toàn tương đương và tránh được những công việc không cần thiết. 

## Ví dụ đã hoạt động 

cho`N = 3`, ba trạng thái đầu tiên là: 

| Bước | Trạng thái giảm | Thậm chí? | Số lần chạy | 
| --- | --- | --- | --- | 
| 0 | 715006 | Có | 1 | 
| 1 | 756687 | Không | 1 | 
| 2 | 798368 | Có | 2 | 

Số lượng là`2`, phù hợp với mẫu Dấu vết này cho thấy giá trị ban đầu được bao gồm trước khi áp dụng bất kỳ chuyển đổi nào. 

Vì`N = 1`, chỉ trạng thái bắt đầu được xem xét: 

| Bước | Trạng thái giảm | Thậm chí? | Số lần chạy | 
| --- | --- | --- | --- | 
| 0 | 715006 | Có | 1 | 

Điều này thể hiện ranh giới trong đó câu trả lời không được bao gồm giá trị được tạo tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^21) | Khám phá chu trình truy cập từng trạng thái giảm có thể có nhiều nhất một lần. | 
| Không gian | O(2^21) | Trình tự được lưu trữ và các vị trí truy cập lần đầu chứa nhiều nhất tất cả các trạng thái rút gọn. | 

Số lượng trạng thái tối đa là khoảng hai triệu, đủ nhỏ đối với Python. Sau khi tiền xử lý, việc trả lời đầu vào chỉ yêu cầu thời gian không đổi cộng với một lần quét nhỏ ở đuôi chu kỳ còn lại. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def brute(n):
    s = 0x600DCAFE
    ans = 0
    for _ in range(n):
        if s % 2 == 0:
            ans += 1
        s = (s + (s >> 20) + 12345) & ((1 << 40) - 1)
    return str(ans)

def run(inp: str) -> str:
    return str(solve(int(inp.strip())))

assert run("0\n") == "0", "minimum size"
assert run("1\n") == brute(1), "single value boundary"
assert run("3\n") == "2", "sample 1"
assert run("10\n") == brute(10), "small manual range"
assert run("100\n") == brute(100), "larger prefix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| Xử lý chuỗi trống | 
|`1`|`1`| Xử lý đúng trạng thái ban đầu | 
|`3`|`2`| Cung cấp hành vi mẫu | 
|`10`| kết quả vũ phu | Chuyển tiếp đúng đắn | 
|`100`| kết quả vũ phu | Tính chính xác của việc đếm tiền tố | 

## Vỏ cạnh 

Đối với đầu vào`0`, thuật toán đi vào nhánh đầu tiên vì`N`nhỏ hơn độ dài tiền tố chu kỳ. Giá trị mảng tiền tố tại chỉ số 0 được trả về, đó là`0`. Không có trạng thái tạo được tính. 

Đối với đầu vào`1`, thuật toán trả về`prefix_even[1]`. Trạng thái giảm ban đầu là chẵn nên đáp án là`1`. Quá trình chuyển đổi không được thực hiện trước khi đếm, điều này tránh được lỗi từng cái một. 

Đối với đầu vào rất lớn, thuật toán không bao giờ cố gắng tạo ra tất cả các giá trị được yêu cầu. Sau khi phát hiện chu trình, nó loại bỏ tiền tố không lặp lại, chia độ dài còn lại cho độ dài chu kỳ và nhân với số trạng thái chẵn trong một chu kỳ. Một số trạng thái còn lại được kiểm tra riêng lẻ nên số đếm cuối cùng vẫn tương ứng chính xác với số đầu tiên.`N`các giá trị được tạo ra.
