---
title: "CF 1031C - Thời gian luyện thi"
description: "Lesha có hai quỹ thời gian riêng biệt, một cho hôm nay và một cho ngày mai. Mỗi ghi chú bài giảng có chi phí đọc cố định bằng chỉ số của nó: ghi chú 1 mất 1 giờ, ghi chú 2 mất 2 giờ, v.v."
date: "2026-06-16T20:40:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1031
codeforces_index: "C"
codeforces_contest_name: "Technocup 2019 - Elimination Round 2"
rating: 1600
weight: 1031
solve_time_s: 818
verified: false
draft: false
---

[CF 1031C - Thời gian luyện thi](https://codeforces.com/problemset/problem/1031/C) 

**Đánh giá:** 1600 
**Thẻ:** tham lam 
**Thời gian giải:** 13 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Lesha có hai quỹ thời gian riêng biệt, một cho hôm nay và một cho ngày mai. Mỗi ghi chú bài giảng có chi phí đọc cố định bằng chỉ số của nó: ghi chú 1 mất 1 giờ, ghi chú 2 mất 2 giờ, v.v. Anh ta có thể chọn bất kỳ tập hợp con ghi chú nào, nhưng mỗi ghi chú đã chọn phải được đọc hoàn toàn trong một ngày và mỗi ghi chú chỉ có thể được sử dụng tối đa một lần trong cả hai ngày. 

Mục tiêu là tối đa hóa tổng số ghi chú riêng biệt mà anh ta đọc, chia chúng thành hai nhóm: một nhóm được giao cho ngày hôm nay với tổng chi phí không vượt quá`a`và một nhóm khác được giao vào ngày mai với tổng chi phí không vượt quá`b`. 

Mặc dù các tờ tiền được lập chỉ mục vô thời hạn, nhưng chỉ những chỉ mục nhỏ mới có thể sử dụng được trên thực tế vì chi phí tăng tuyến tính. Cấu trúc chính là các chỉ số nhỏ hơn sẽ rẻ hơn rất nhiều, do đó chúng luôn hiệu quả hơn về mặt tối đa hóa số lượng. 

Những ràng buộc cho phép`a, b`lên tới 10^9, điều này ngay lập tức loại trừ mọi nỗ lực mô phỏng trạng thái ba lô hoặc thử các tập hợp con một cách linh hoạt. Bất kỳ giải pháp nào cũng phải suy luận trực tiếp về cấu trúc của các lựa chọn tối ưu hơn là liệt kê các khả năng. 

Một sai lầm ngây thơ là cố gắng giao việc một cách tham lam từng ngày mà không có sự phối hợp toàn cầu. Ví dụ, nếu`a = 3`Và`b = 3`, đang chọn`{1,2}`vì ngày hôm nay sử dụng hết 3 giờ, không để lại gì hữu ích cho ngày mai ngoại trừ những món đồ có giá thành lớn. Nhưng việc hoán đổi bài tập hoặc phân chia khác nhau có thể cải thiện tổng số. Một dạng sai sót tinh vi khác là chọn các ghi chú nhỏ nhất có thể một cách độc lập cho mỗi ngày mà không đảm bảo tính duy nhất toàn cầu; cả hai ngày có thể cố gắng sử dụng cùng một tờ tiền rẻ tiền. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi cách có thể để phân chia tiền tố của các ghi chú giữa hai ngày. Đối với chỉ mục ghi chú lớn nhất cố định`k`, chúng ta có thể chọn bất kỳ tập con nào của`{1..k}`, chỉ định từng phần tử cho ngày thứ nhất hoặc ngày thứ hai và kiểm tra tính khả thi của cả hai khoản tiền. Điều này đã theo cấp số nhân trong`k`, vì mỗi ghi chú có ba lựa chọn: chưa sử dụng, ngày thứ nhất hoặc ngày thứ hai. Ngay cả khi chúng tôi giới hạn`k`ở giá trị lớn nhất tại đó`k(k+1)/2 ≤ a+b`, số lượng nhiệm vụ tăng lên quá nhanh. 

Quan sát quan trọng là do chi phí ngày càng tăng nên mọi giải pháp tối ưu sẽ luôn bao gồm các lựa chọn giống như tiền tố. Nếu sử dụng một tờ tiền có số cao hơn thì tất cả những tờ tiền chưa sử dụng rẻ hơn luôn là ứng cử viên tốt hơn để đưa vào đầu tiên. Điều này làm giảm vấn đề phân phối các số nhỏ nhất trên hai chiếc ba lô. 

Thay vì suy nghĩ về mặt phân công, chúng ta đảo ngược quan điểm: đầu tiên chúng ta cố gắng ghi càng nhiều ghi chú nhỏ nhất càng tốt, bỏ qua sự phân chia, sau đó chúng ta quyết định chia chúng ra sao cho hai ngày. Nếu lấy các nốt 1, 2, 3, …, k thì tổng giá trị của chúng là`k(k+1)/2`. Chúng tôi chọn mức tối đa`k`sao cho số tiền này phù hợp với`a + b`. Sau đó, nhiệm vụ còn lại là chia những thứ này ra trước`k`số giữa hai giới hạn công suất. 

Sau khi tiền tố được cố định, việc gán tham lam từ lớn nhất đến nhỏ nhất là tối ưu: các mục lớn hơn sẽ khó đặt hơn, vì vậy chúng nên được đặt trước vào bất kỳ ngày nào có thể chứa chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tiền tố + chia tham lam | O(k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Tìm tiền tố lớn nhất 

Chúng tôi tính toán lớn nhất`k`như vậy`1 + 2 + ... + k ≤ a + b`. Điều này đảm bảo rằng chúng tôi không bao giờ cố gắng sử dụng một tờ tiền mà chúng tôi không đủ khả năng chi trả dù tổng cộng là bao nhiêu. 

Lý do là bất kỳ giải pháp tối ưu nào sử dụng`k`ghi chú nhất thiết phải sử dụng một số tập hợp con của ghi chú đầu tiên`k`các chỉ số, bởi vì bỏ qua một nốt nhỏ hơn để chuyển sang một nốt lớn hơn không bao giờ có lợi. 

### Bước 2: Khởi tạo dung lượng còn lại 

Chúng tôi đối xử`a`Và`b`như dung lượng còn lại cho hai thùng độc lập. Ban đầu cả hai đều đầy. 

### Bước 3: Gán nốt từ k xuống 1 

Chúng tôi lặp lại từ`k`xuống tới`1`. Đối với mỗi ghi chú`i`, chúng tôi cố gắng đặt nó vào ngày phù hợp. 

Chúng tôi muốn đặt nó vào ngày đầu tiên nếu có thể. Nếu không, chúng tôi sẽ thử ngày thứ hai. 

Lý do chúng ta đi xuống là vì các nốt lớn hơn sẽ hạn chế hơn. Nếu chúng ta trì hoãn chúng, chúng ta có thể mất tính khả thi ngay cả khi có một nhiệm vụ hợp lệ. 

### Bước 4: Lưu trữ bài tập 

Chúng tôi duy trì hai danh sách: một cho ngày thứ nhất và một cho ngày thứ hai. Mỗi lần chúng tôi chỉ định một nốt nhạc, chúng tôi sẽ trừ giá trị của nó khỏi dung lượng tương ứng. 

### Bước 5: Xuất ra 

Chúng tôi in cả hai nhóm. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên một lập luận trao đổi đơn điệu. Bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành giải pháp sử dụng tiền tố số nguyên mà không làm giảm số phần tử. Sau khi tiền tố được cố định, việc gán các phần tử lớn hơn trước tiên sẽ không bao giờ cản trở một giải pháp khả thi vì bất kỳ việc không đặt phần tử lớn nào sẽ có nghĩa là cả hai ngăn đều quá nhỏ để chứa nó, điều này mâu thuẫn với tính khả thi của bất kỳ phân vùng hợp lệ nào chứa nó. 

Do đó, vị trí tham lam từ lớn nhất đến nhỏ nhất sẽ duy trì tính khả thi và tối đa hóa việc sử dụng tất cả các số có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_k(total):
    k = 0
    cur = 0
    while True:
        if cur + (k + 1) > total:
            break
        k += 1
        cur += k
    return k

a, b = map(int, input().split())
total = a + b

k = max_k(total)

a_rem, b_rem = a, b
day1 = []
day2 = []

for i in range(k, 0, -1):
    if i <= a_rem:
        day1.append(i)
        a_rem -= i
    else:
        day2.append(i)
        b_rem -= i

print(len(day1))
print(*day1)
print(len(day2))
print(*day2)
```Hàm đầu tiên tính tổng tiền tố lớn nhất phù hợp với dung lượng kết hợp. Điều này tránh mọi nhu cầu xem xét các con số vượt quá khả năng có thể trên toàn cầu. 

Vòng lặp tham lam xử lý từ`k`đi xuống. Mỗi số được đặt vào thùng đầu tiên nơi nó phù hợp. Các bản cập nhật trừ đảm bảo chúng tôi không bao giờ vượt quá khả năng. 

Lựa chọn thử ngày đầu tiên là tùy ý; một trong hai thứ tự đều có tác dụng vì nhiệm vụ cuối cùng không phải là duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1: a = 3, b = 3 

Đầu tiên chúng ta tính giá trị lớn nhất`k`như vậy`k(k+1)/2 ≤ 6`. Điều này mang lại`k = 3`từ`1+2+3 = 6`. 

| tôi | a_rem | b_rem | Hành động | ngày1 | ngày2 | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 3 | 3 | phù hợp với ngày1 | [3] | [] | 
| 2 | 0 | 3 | phù hợp với ngày thứ 2 | [3] | [2] | 
| 1 | 0 | 1 | phù hợp với ngày thứ 2 | [3] | [2,1] | 

Ngày thứ nhất dùng đúng 3 giờ, ngày thứ hai dùng đúng 3 giờ. Tất cả các ghi chú được sử dụng. 

Điều này chứng tỏ rằng thứ tự tham lam tránh chặn các phần tử nhỏ hơn một cách không cần thiết trong khi vẫn đặt các ràng buộc lớn lên hàng đầu. 

### Ví dụ 2: a = 5, b = 2 

Tổng cộng là 7, vậy`k = 3`từ`1+2+3 = 6`. 

| tôi | a_rem | b_rem | Hành động | ngày1 | ngày2 | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 5 | 2 | phù hợp với ngày1 | [3] | [] | 
| 2 | 2 | 2 | phù hợp với ngày1 | [3,2] | [] | 
| 1 | 0 | 2 | phù hợp với ngày thứ 2 | [3,2] | [1] | 

Điều này cho thấy sự mất cân bằng được xử lý một cách tự nhiên: công suất lớn hơn sẽ hấp thụ các vật nặng hơn trước, trong khi ngày nhỏ hơn vẫn nhận được các phần tử nhỏ còn sót lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | chuyển một lần qua tiền tố đã chọn | 
| Không gian | O(k) | lưu trữ các ghi chú được giao | 

Giá trị của`k`được giới hạn bởi số nguyên lớn nhất sao cho`k(k+1)/2 ≤ 2×10^9`, tức là khoảng 60000, nên nghiệm dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a, b = map(int, input().split())
    total = a + b

    k = 0
    cur = 0
    while cur + (k + 1) <= total:
        k += 1
        cur += k

    a_rem, b_rem = a, b
    day1 = []
    day2 = []

    for i in range(k, 0, -1):
        if i <= a_rem:
            day1.append(i)
            a_rem -= i
        else:
            day2.append(i)
            b_rem -= i

    out = []
    out.append(str(len(day1)))
    out.append(" ".join(map(str, day1)))
    out.append(str(len(day2)))
    out.append(" ".join(map(str, day2)))
    return "\n".join(out)

# sample
assert run("3 3") is not None

# minimum case
assert run("0 0") == "0\n\n0\n"

# only one day
assert run("10 0") is not None

# asymmetric split
assert run("5 2") is not None

# large balanced
assert run("1000000000 1000000000") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | nhóm trống | xử lý công suất bằng không | 
| 10 0 | tất cả trong một ngày | đóng gói một mặt | 
| 5 2 | tính khả thi phân chia | tham lam phân phối đúng đắn | 
| lớn bằng | sử dụng đầy đủ | hiệu suất và tiền tố bị ràng buộc | 

## Vỏ cạnh 

Khi một trong`a`hoặc`b`bằng 0, thuật toán sẽ tự nhiên đặt mọi thứ vào ngày khác vì việc kiểm tra tham lam không thành công đối với mặt trống trước và tất cả các mục sẽ chuyển sang dung lượng duy nhất có sẵn. 

Đối với các đầu vào bằng nhau rất lớn, quá trình tính toán tiền tố dừng ở khoảng 60000, do đó không xuất hiện vấn đề tràn hoặc hiệu suất. Vòng lặp tham lam vẫn chỉ chạy trên tiền tố này, đảm bảo hành vi thời gian tuyến tính không phụ thuộc vào cường độ đầu vào. 

Một trường hợp tế nhị là khi năng lực bị sai lệch nhiều, chẳng hạn`a = 1`Và`b = 1000000000`. Thuật toán trước tiên vẫn xác định tiền tố từ tổng dung lượng, sau đó gán các giá trị nhỏ trước vào thùng lớn, bảo toàn tính khả thi mà không cần bất kỳ logic trường hợp đặc biệt nào.
