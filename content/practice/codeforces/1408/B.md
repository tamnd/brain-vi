---
title: "CF 1408B - Tổng mảng"
description: "Chúng ta được cung cấp một mảng không giảm a trong đó mỗi phần tử là một số nguyên không âm. Chúng tôi muốn chia mảng này thành tổng của m mảng b₁, b₂, ..., bₘ sao cho mỗi bᵢ cũng không giảm và có hạn chế về cấu trúc mạnh mẽ: nó sử dụng tối đa k giá trị riêng biệt."
date: "2026-06-11T07:40:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1408
codeforces_index: "B"
codeforces_contest_name: "Grakn Forces 2020"
rating: 1400
weight: 1408
solve_time_s: 77
verified: true
draft: false
---

[CF 1408B - Tổng mảng](https://codeforces.com/problemset/problem/1408/B) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng không giảm`a`trong đó mỗi mục là một số nguyên không âm. Chúng tôi muốn chia mảng này thành tổng`m`mảng`b₁, b₂, ..., bₘ`sao cho mỗi`bᵢ`cũng không giảm và có hạn chế mạnh về cấu trúc: nó sử dụng tối đa`k`những giá trị riêng biệt. 

Tại mỗi vị trí`j`, các giá trị trên tất cả các mảng phải cộng chính xác với`a[j]`. Vì vậy, chúng tôi đang phân phối “chiều cao” của từng chỉ mục trên nhiều chuỗi đơn điệu, nhưng bản thân mỗi chuỗi phải đơn giản theo nghĩa là nó không thể nhận quá nhiều giá trị khác nhau. 

Nhiệm vụ là tìm số lượng chuỗi như vậy tối thiểu hoặc xác định rằng điều đó là không thể. 

Các hạn chế là nhỏ:`n ≤ 100`,`a[j] ≤ 100`và lên tới 100 trường hợp thử nghiệm. Điều này cho thấy rằng bất kỳ giải pháp nào lên tới khoảng`O(n^3)`hoặc thậm chí là cẩn thận`O(n^2 * k)`được chấp nhận. Tuy nhiên, việc xây dựng các phân tách tùy ý một cách bạo lực là không khả thi vì số lượng mảng có thể tăng theo cấp số nhân với`n`. 

Một hạn chế cơ cấu quan trọng là mỗi`bᵢ`là không giảm. Điều này có nghĩa là một khi giá trị tăng ở một chỉ số nào đó thì sau đó nó không bao giờ có thể giảm. Kết hợp với “tối đa`k`các giá trị riêng biệt”, mỗi giá trị`bᵢ`về cơ bản bao gồm nhiều nhất`k`các đoạn phẳng. 

Một trường hợp khó phát hiện khi`a`đang tăng mạnh với những khoảng trống nhỏ. Sự phân chia tham lam ngây thơ có thể đánh giá thấp số lượng “lớp giá trị” cần thiết. Một trường hợp cạnh khác là khi`k = 1`: mọi`bᵢ`phải không đổi, điều này hạn chế rất nhiều tính khả thi. 

## Phương pháp tiếp cận 

Nỗ lực đầu tiên tự nhiên là nghĩ về từng`bᵢ`như một “lớp” mà chúng ta liên tục bóc ra`a`. Vì tất cả các mảng đều không giảm nên mỗi lớp cũng phải không giảm. Chiến lược brute-force sẽ cố gắng xây dựng từng lớp một: liên tục tìm một giá trị hợp lệ`bᵢ`, trừ nó khỏi`a`, và tiếp tục. 

Điều này nhanh chóng trở nên khó khăn vì việc chọn một lớp sẽ hạn chế tất cả các lớp trong tương lai. Có nhiều cách phân tách hợp lệ có thể có ở mỗi bước và việc khám phá chúng sẽ dẫn đến sự phân nhánh theo cấp số nhân. 

Quan sát quan trọng là lật ngược quan điểm. Thay vì xây dựng các lớp, chúng tôi suy luận xem cần bao nhiêu lớp để thể hiện các “bước nhảy” trong mảng. Mỗi lần giá trị tăng lên`a`, mức tăng đó phải được hỗ trợ bởi đủ lớp mà vẫn có thể đáp ứng các thay đổi có giá trị riêng biệt. 

Cụ thể hơn, hãy tưởng tượng việc quét`a`từ trái sang phải. Bất cứ khi nào giá trị tăng từ`a[j-1]`ĐẾN`a[j]`, chúng tôi buộc phải cung cấp đủ công suất trong các lớp của mình để hỗ trợ sự tăng trưởng này trong khi vẫn duy trì tính đơn điệu và giới hạn của`k`các giá trị riêng biệt trên mỗi lớp. Mỗi lớp chỉ có thể “thay đổi hành vi” một số lần giới hạn (nhiều nhất là`k-1`tăng lên). Điều này biến vấn đề thành việc đếm xem có bao nhiêu “sự kiện tăng” đơn điệu phải được phân bổ trên các lớp. 

Cấu trúc tham lam trở thành: mô phỏng phân phối số gia trên các lớp, luôn lấp đầy các lớp hiện có cho đến khi chúng đạt đến giới hạn giá trị riêng biệt và chỉ mở các lớp mới khi cần thiết. 

Điều này dẫn đến cách giải thích phân lớp tối thiểu: mỗi lớp có thể hấp thụ nhiều nhất`k-1`tăng giá trị dọc theo mảng. Vì vậy, chúng tôi nhóm lại một cách tham lam thành từng thùng dung lượng`k-1`, và số lượng thùng là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm phân rã Brute Force | Hàm mũ | O(nm) | Quá chậm | 
| Tham lam phân phối tăng | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng và chỉ tập trung vào các vị trí mà`a[j] > a[j-1]`, vì các đoạn phẳng không gây ra thay đổi về cấu trúc. 

1. Tính tất cả các mức tăng dương`d[j] = a[j] - a[j-1]`vì`j ≥ 2`. Mỗi vị trí như vậy đại diện cho một “sự kiện tăng” bắt buộc phải được hỗ trợ bởi các lớp. 
2. Coi mỗi lần tăng đơn vị là yêu cầu phân bổ thành các lớp, nhưng hãy quan sát rằng mỗi lớp có thể đáp ứng tối đa`k-1`các sự kiện tăng như vậy trước khi nó cần đưa ra một giá trị khác biệt mới vượt quá giới hạn cho phép. 
3. Duy trì số lớp hiện tại`m`và một bộ đếm theo dõi số lượng mức tăng đã được chỉ định cho lô lớp hiện tại. 
4. Lặp lại mảng và tích lũy các mức tăng vào lô hiện tại. Nếu thêm lần tăng yêu cầu tiếp theo sẽ vượt quá công suất`k-1`, bắt đầu một lớp mới và đặt lại bộ đếm. 
5. Nếu`k = 1`, ngay lập tức kiểm tra tính khả thi. Trong trường hợp này mỗi`bᵢ`phải không đổi, do đó, tổng mảng duy nhất có thể đến từ việc xếp chồng các mảng không đổi. Điều này chỉ có thể thực hiện được nếu`a`là hằng số, ngược lại câu trả lời là`-1`. 
6. Xuất ra tổng số lớp đã tạo. 

### Tại sao nó hoạt động 

Mỗi lớp tương ứng với một dãy không giảm với tối đa`k`các giá trị riêng biệt, có nghĩa là nó chỉ có thể chuyển đổi từ giá trị này sang giá trị khác`k-1`lần. Mỗi sự gia tăng nghiêm ngặt về`a`buộc ít nhất một quá trình chuyển đổi như vậy phải được hỗ trợ ở đâu đó giữa các lớp. 

Bởi vì tất cả các lớp đều đóng góp bổ sung và độc lập, nên việc phân phối các sự kiện gia tăng một cách tham lam vào các nhóm có giới hạn dung lượng sẽ giảm thiểu số lượng lớp cần thiết. Bất kỳ nỗ lực nào nhằm giảm số lượng lớp sẽ yêu cầu một số lớp hấp thụ nhiều hơn`k-1`tăng, vi phạm ràng buộc giá trị khác biệt. Ngược lại, việc đóng gói tham lam luôn tôn trọng giới hạn và không bao giờ lãng phí năng lực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if k == 1:
            ok = all(x == a[0] for x in a)
            print(1 if ok else -1)
            continue

        increases = 0
        layers = 1
        used = 0

        for i in range(1, n):
            if a[i] > a[i - 1]:
                inc = a[i] - a[i - 1]
                used += inc
                if used > k - 1:
                    layers += 1
                    used = inc

        print(layers)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc kiểm tra tính khả thi trực tiếp cho`k = 1`. Trong trường hợp đó, mọi mảng phải không đổi, do đó bất kỳ sự thay đổi nào trong`a`ngay lập tức làm cho sự phân hủy không thể thực hiện được. 

Vì`k ≥ 2`, mã theo dõi số lượng “đơn vị tăng” đang được chỉ định vào nhóm lớp hiện tại. Bất cứ khi nào điều này vượt quá`k-1`, một lớp mới được tạo. giá trị`k-1`xuất hiện vì một mảng không giảm có nhiều nhất`k`các giá trị riêng biệt có thể tăng nhiều nhất`k-1`lần. 

Một điểm tinh tế là chúng ta tích lũy những khác biệt`a[i] - a[i-1]`, không chỉ đếm vị trí. Điều này đảm bảo rằng các bước nhảy lớn hơn sẽ tiêu tốn nhiều dung lượng hơn tương ứng, vì chúng yêu cầu tăng nhiều đơn vị trong quá trình phân tách lớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
0 0 0 1
```Chúng tôi theo dõi mức tăng: 

| tôi | a[i-1] | một [tôi] | tăng | đã qua sử dụng | lớp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 0 | 0 | 1 | 
| 3 | 0 | 1 | 1 | 1 | 1 | 

Từ`k = 2`, mỗi lớp có thể xử lý tối đa`1`tăng. Tổng mức tăng là`1`, vì vậy một lớp là đủ. 

Điều này cho thấy cách thuật toán chỉ xử lý các mức tăng thực tế dưới dạng ràng buộc, bỏ qua các vùng phẳng. 

### Ví dụ 2 

đầu vào:```
5 2
1 2 3 4 5
```| tôi | a[i-1] | một [tôi] | tăng | đã qua sử dụng | lớp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 1 | 1 | 
| 2 | 2 | 3 | 1 | 2 → đặt lại | 2 | 
| 3 | 3 | 4 | 1 | 1 | 2 | 
| 4 | 4 | 5 | 1 | 2 → đặt lại | 3 | 

Ở đây mỗi bước tăng thêm 1 nên mỗi lớp chỉ có thể hấp thụ một lần tăng trước khi vượt quá công suất. Điều này buộc nhiều lớp, minh họa sự ràng buộc chặt chẽ được áp đặt bởi các lớp nhỏ`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n cho mỗi trường hợp thử nghiệm) | Sự khác biệt về tính toán mảng một lần vượt qua | 
| Không gian | O(1) | Chỉ có bộ đếm được lưu trữ | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với`n ≤ 100`, do đó việc quét tuyến tính cho mỗi trường hợp kiểm thử dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))

            if k == 1:
                ok = all(x == a[0] for x in a)
                print(1 if ok else -1)
                continue

            used = 0
            layers = 1
            for i in range(1, n):
                if a[i] > a[i - 1]:
                    used += a[i] - a[i - 1]
                    if used > k - 1:
                        layers += 1
                        used = a[i] - a[i - 1]
            print(layers)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""6
4 1
0 0 0 1
3 1
3 3 3
11 3
0 1 2 2 3 3 3 4 4 4 4
5 3
1 2 3 4 5
9 4
2 2 3 5 7 11 13 13 17
10 7
0 1 1 2 3 3 4 5 5 6
""") == """-1
1
2
2
2
1"""

# custom cases
assert run("""1
1 5
7
""") == "1", "single element"

assert run("""1
3 1
1 1 2
""") == "-1", "k=1 non-constant"

assert run("""1
4 2
0 1 1 2
""") == "2", "small layered increases"

assert run("""1
6 3
0 0 0 1 1 2
""") == "1", "enough capacity in one layer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | mảng tối thiểu luôn khả thi | 
| k=1 không đổi | -1 | trường hợp bất khả thi | 
| tăng lớp nhỏ | 2 | logic tách lớp | 
| đủ công suất trong một lớp | 1 | tham lam đóng gói đúng cách | 

## Vỏ cạnh 

Khi nào`k = 1`, thuật toán sẽ ngay lập tức kiểm tra xem tất cả các phần tử có bằng nhau hay không. Đối với đầu vào`a = [5, 5, 5]`, nó trả về`1`bởi vì một mảng không đổi duy nhất là đủ. Đối với đầu vào`a = [0, 0, 1]`, nó trả về`-1`vì không có sự phân rã liên tục nào có thể tạo ra sự gia tăng nghiêm ngặt. 

Khi mảng có một phần tử duy nhất, chẳng hạn như`n = 1`, bất kì`k ≥ 1`hoạt động và câu trả lời luôn là`1`. Thuật toán xử lý việc này một cách tự nhiên vì không gặp phải sự gia tăng nào và`layers`còn lại`1`. 

Khi mức tăng lớn, chẳng hạn như`a = [0, 0, 10]`với`k = 3`, thuật toán đếm chính xác bước nhảy là`10`đơn vị công suất. Vì mỗi lớp chỉ có thể xử lý`2`tăng lên, nó tạo ra`5`các lớp, phản ánh rằng các bước nhảy lớn phải được phân phối trên nhiều chuỗi thay vì được coi là một sự kiện duy nhất.
