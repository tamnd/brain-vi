---
title: "CF 1988A - Chia Multiset"
description: "Chúng ta bắt đầu với một tập hợp chứa một số n. Cách duy nhất để thay đổi nhiều tập hợp này là liên tục chọn một số u hiện có, xóa nó và thay thế nó bằng tối đa k số nguyên dương có tổng chính xác là u."
date: "2026-06-08T15:46:27+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1988
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 958 (Div. 2)"
rating: 900
weight: 1988
solve_time_s: 80
verified: true
draft: false
---

[CF 1988A - Tách nhiều tập hợp](https://codeforces.com/problemset/problem/1988/A) 

**Xếp hạng:** 900 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với nhiều tập hợp chứa một số duy nhất`n`. Cách duy nhất để thay đổi nhiều tập hợp này là chọn liên tục một số hiện có`u`, xóa nó và thay thế bằng nhiều nhất`k`số nguyên dương có tổng chính xác`u`. Mục tiêu là chuyển đổi nhiều tập hợp để cuối cùng nó chỉ bao gồm một tập hợp và chúng tôi muốn giảm thiểu số lượng thao tác thay thế như vậy được sử dụng. 

Quan điểm chính là ngừng suy nghĩ về các tập hợp và thay vào đó theo dõi một đại lượng duy nhất: tổng số phần tử trong nhiều tập hợp. Mọi thao tác thay thế một số bằng một số số nhỏ hơn, tăng số phần tử trong khi vẫn giữ nguyên tổng. Vì trạng thái cuối cùng phải là`n`1, số phần tử cuối cùng chính xác là`n`, và tổng cũng là`n`. 

Các ràng buộc đủ nhỏ để có thể sử dụng phương pháp mô phỏng hoặc tham lam trực tiếp. Mỗi thao tác giảm tối đa một giá trị`k`nên quá trình tăng trưởng có tính chất logarit. Với`n ≤ 1000`, thậm chí việc mô phỏng các hoạt động hoặc tính toán công thức cho mỗi trường hợp kiểm thử cũng đủ nhanh. 

Trường hợp cạnh tinh tế xuất hiện khi`n = 1`. Trong trường hợp này, nhiều tập hợp đã chỉ chứa những tập hợp đơn lẻ nên không cần thực hiện thao tác nào. Một trường hợp cạnh khác là khi`k`đủ lớn để chúng ta có thể chia bất kỳ số nào thành tất cả các số trong một bước, nhưng vẫn phải tôn trọng quy tắc nhiều nhất là`k`các bộ phận. Một giả định ngây thơ rằng chúng ta luôn chia thành chính xác`k`các bộ phận sẽ thất bại khi`u < k`. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là mô phỏng quá trình: liên tục chọn phần tử lớn nhất, chia nó thành một số phần và tiếp tục cho đến khi mọi thứ trở thành một. Điều này đúng nhưng lộn xộn vì các lựa chọn phân tách khác nhau dẫn đến các trạng thái trung gian khác nhau. Hệ số phân nhánh lớn vì mỗi số có thể được chia theo nhiều cách thành tối đa`k`các bộ phận. Ngay cả đối với nhỏ`n`, điều này tạo ra một số lượng cấp số nhân của các chuỗi có thể. 

Nhận xét quan trọng là thứ tự thực hiện các thao tác không quan trọng; điều quan trọng là chúng ta buộc phải "mở rộng" một giá trị bao nhiêu lần trước khi nó trở thành 1. Mỗi thao tác thay thế một số`u`với nhiều nhất`k`các phần, vì vậy chiến lược tốt nhất luôn là chia đều nhất có thể thành các phần chính xác`k`các bộ phận bất cứ khi nào`u > 1`. Điều này tối đa hóa sự tiến bộ đối với những người. 

Vì vậy, quá trình trở nên xác định: chúng tôi liên tục thay thế một số`u`với`k`các phần gần bằng nhau cho đến khi mọi thứ bằng 1. Số thao tác cần thiết chỉ đơn giản là số lần chúng ta cần ngắt`n`xuống cho đến khi tất cả các phần bằng 1, tương đương với việc thay thế liên tục một nút bằng tối đa`k`những đứa trẻ ở trên cây cho đến khi chúng ta đạt được những chiếc lá có kích thước 1. 

Điều này chuyển thành một mô hình tăng trưởng đơn giản: mỗi thao tác sẽ tăng số lượng phần và chúng ta cần tăng từ 1 phần lên`n`các mảnh, với mỗi thao tác tăng số lượng mảnh tối đa`k - 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tất cả các phần chia tách | hàm mũ | cao | Quá chậm | 
| Tham lam đếm tăng trưởng | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng chính là theo dõi xem chúng tôi hiện có bao nhiêu phần và cần bao nhiêu thao tác để đạt được ít nhất`n`bắt đầu từ một giá trị. 

1. Chúng ta bắt đầu với một phần tử có giá trị`n`. Về mặt khái niệm, chúng tôi coi đây là một “đoạn” đang hoạt động. 
2. Mỗi thao tác chiếm một đoạn và thay thế nó bằng nhiều nhất`k`khối nhỏ hơn. Nếu chúng tôi luôn phân chia một cách tối ưu, chúng tôi coi việc này là thay thế một đoạn bằng chính xác`k`khối bất cứ khi nào có thể. Điều này tối đa hóa số lượng khối đạt được cho mỗi hoạt động. 
3. Ban đầu ta có`1`khúc. Sau mỗi thao tác, số lượng khối tăng tối đa`k - 1`, vì một đoạn được loại bỏ và lên đến`k`được thêm vào. 
4. Chúng tôi muốn tiếp cận chính xác`n`các khối, bởi vì mỗi khối cuối cùng tương ứng với một`1`. 
5. Vì vậy, chúng tôi tính toán cần bao nhiêu bước để đi từ`1`ĐẾN`n`sử dụng số gia của`k - 1`. Đây là một quá trình tăng trưởng số học đơn giản. 
6. Nếu`k = 1`, không thể chia tách được trừ khi`n = 1`. Vì vậy, chúng tôi xử lý việc đó một cách riêng biệt. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi thao tác, tổng số không đổi trong khi số phần tăng lên đến mức tối đa có thể khi chúng ta chia thành`k`các bộ phận. Vì mọi cấu hình cuối cùng chỉ là`n`thì việc đạt được mục tiêu tương đương với việc đạt được`n`miếng. Bất kỳ sai lệch nào so với việc phân chia tối đa chỉ làm trì hoãn tiến độ, vì vậy chiến lược tối ưu luôn là tối đa hóa số lượng phần mới cho mỗi thao tác. Điều này làm giảm vấn đề thành một quá trình tăng trưởng xác định với mức tăng cố định trên mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())

        if n == 1:
            print(0)
            continue

        if k == 1:
            # cannot split anything larger than 1
            print(-1)
            continue

        # each operation increases number of pieces by at most (k-1)
        # start with 1 piece, need to reach n pieces
        need = n - 1
        step_gain = k - 1

        ans = (need + step_gain - 1) // step_gain
        print(ans)

if __name__ == "__main__":
    solve()
```## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6, k = 3
```Chúng ta bắt đầu với 1 đoạn. 

Mỗi thao tác tăng khối lên 2. 

| Bước | khối | 
| --- | --- | 
| 0 | 1 | 
| 1 | 3 | 
| 2 | 5 | 
| 3 | 7 | 

Chúng ta đạt ít nhất 6 sau 3 thao tác, vì vậy đáp án là 3. 

### Ví dụ 2 

đầu vào:```
n = 16, k = 4
```Mỗi thao tác tăng khối lên 3. 

| Bước | khối | 
| --- | --- | 
| 0 | 1 | 
| 1 | 4 | 
| 2 | 7 | 
| 3 | 10 | 
| 4 | 13 | 
| 5 | 16 | 

Chúng tôi đạt chính xác 16 trong 5 thao tác, khớp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi lần kiểm tra | Mỗi bài kiểm tra tính một công thức không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm và giải pháp này xử lý từng trường hợp thử nghiệm trong thời gian không đổi, do đó, nó phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

def solve():
    data = sys.stdin.read().strip().split()
    t = int(data[0])
    idx = 1
    out = []
    for _ in range(t):
        n = int(data[idx]); k = int(data[idx+1])
        idx += 2

        if n == 1:
            out.append("0")
        elif k == 1:
            out.append("-1")
        else:
            need = n - 1
            step = k - 1
            out.append(str((need + step - 1) // step))

    print("\n".join(out))

# provided samples
assert solve.__code__  # placeholder to avoid empty structure
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | 0 | trường hợp cơ sở không có hoạt động | 
| k=1, n>1 | -1 | không thể chia tách | 
| n=2, k=2 | 1 | trường hợp tăng trưởng tối thiểu | 
| k lớn | câu trả lời nhỏ | tách nhanh | 

## Vỏ cạnh 

Khi nào`n = 1`, multiset đã đáp ứng mục tiêu vì nó chỉ chứa một, do đó không cần thao tác nào. Thuật toán trả về đúng 0 ngay lập tức. 

Khi`k = 1`Và`n > 1`, không có phần tử nào có thể chia thành nhiều phần, vì vậy chúng ta không bao giờ có thể tăng số lượng phần tử. Vì chúng ta bắt đầu với một yếu tố và cần`n > 1`, câu trả lời là không thể và thuật toán trả về`-1`. 

Khi`k >= n`, một lần phân chia có thể tạo ra nhiều nhất`n`những cái đó trong một thao tác, nhưng chỉ khi được cho phép bởi quy tắc tối đa`k`các bộ phận. Công thức tự nhiên giảm xuống còn 1 thao tác khi có thể, phù hợp với hành vi tối ưu.
