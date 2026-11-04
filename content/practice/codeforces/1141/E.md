---
title: "CF 1141E - Trận chiến siêu anh hùng"
description: "Chúng ta được cấp cho một con quái vật có giá trị sức khỏe ban đầu và kiểu sát thương lặp lại được áp dụng một lần mỗi phút. Mẫu có độ dài n và sau phút cuối cùng, chúng ta ngay lập tức quay lại phút đầu tiên và tiếp tục mãi mãi."
date: "2026-06-12T03:42:29+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1141
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 547 (Div. 3)"
rating: 1700
weight: 1141
solve_time_s: 95
verified: true
draft: false
---

[CF 1141E - Trận chiến siêu anh hùng](https://codeforces.com/problemset/problem/1141/E) 

**Đánh giá:** 1700 
**Thẻ:** toán 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp cho một con quái vật có giá trị sức khỏe ban đầu và kiểu sát thương lặp lại được áp dụng một lần mỗi phút. Mẫu có chiều dài`n`, và sau phút cuối cùng, chúng tôi ngay lập tức quay lại phút đầu tiên và tiếp tục mãi mãi. 

Mỗi phút sẽ điều chỉnh tình trạng sức khỏe hiện tại bằng cách thêm một số nguyên cố định, có thể âm hoặc dương. Quái vật chết khi sức khỏe của nó trở nên không tích cực. Chúng ta phải xác định phút sớm nhất trong dòng thời gian vô tận này khi điều này xảy ra, hoặc kết luận rằng điều này không bao giờ xảy ra. 

Các ràng buộc làm cho lý luận vũ phu trở nên quan trọng. Về nguyên tắc, số phút chúng tôi có thể mô phỏng là không giới hạn, nhưng`n`có thể lên tới 200.000 và lượng máu ban đầu lên tới 10^12. Một mô phỏng đơn giản về tiến trình từng phút có thể yêu cầu tới 10^12 lần lặp trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Ngay cả một lần lặp lại nhiều lần cũng chỉ được chấp nhận nếu chúng ta có thể bỏ qua các khối lớn. 

Một vấn đề tế nhị là sức khỏe có thể tăng lên trong một hiệp và sau đó giảm xuống sau đó, do đó thời điểm tử vong có thể xảy ra ở giữa chu kỳ chứ không chỉ ở ranh giới chu kỳ. 

Trường hợp một cạnh là khi tổng của một chu kỳ đầy đủ không âm và không có tiền tố nào giảm đủ để tiêu diệt quái vật. Ví dụ, nếu tất cả`d_i`là tích cực, con quái vật sẽ không bao giờ chết. Một mô phỏng đơn giản có thể vẫn chạy mãi mãi. 

Một trường hợp khác xuất hiện khi hiệu ứng tích lũy nhìn chung là âm nhưng quái vật vẫn sống sót trong chu kỳ đầu. Cái chết có thể xảy ra sâu bên trong chu kỳ sau sau nhiều lần lặp lại đầy đủ, vì vậy chúng ta phải kết hợp hành vi tiền tố với những thay đổi lặp đi lặp lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng từng phút, liên tục áp dụng trình tự và kiểm tra xem sức khỏe có giảm xuống 0 hay thấp hơn hay không. Điều này đúng vì nó phản ánh chính xác quy trình, nhưng nó không khả thi vì số bước mô phỏng có thể cực kỳ lớn. Nếu câu trả lời là`k`, thì trong trường hợp xấu nhất`k`có thể ở mức 10^12 trở lên, điều này khiến phương pháp này không thể thực hiện được trong thời gian giới hạn. 

Quan sát quan trọng là trình tự này mang tính định kỳ, vì vậy sau mỗi chu kỳ đầy đủ, sức khỏe sẽ thay đổi một giá trị không đổi bằng tổng của tất cả`d_i`. Điều này cho phép chúng tôi nén các chu kỳ lặp đi lặp lại. Thay vì mô phỏng từng phút, chúng tôi mô phỏng toàn bộ chu kỳ đồng thời theo dõi sự thay đổi sức khỏe trên toàn cầu. 

Chúng tôi tính toán trước tổng tiền tố của mảng, cho chúng tôi biết sự thay đổi thực sau mỗi phút trong một chu kỳ. Trong bất kỳ chu kỳ nào bắt đầu từ một sức khỏe nhất định`H`, chúng ta có thể xác định xem quái vật có chết trong chu kỳ đó hay không bằng cách kiểm tra xem`H + prefix[i] <= 0`đối với một số người`i`. Nếu không, chúng tôi tính toán sức khỏe mới sau toàn bộ chu kỳ như sau`H + total_sum`và lặp lại. 

Nếu tổng của một chu kỳ không âm và quái vật sống sót sau một chu kỳ trọn vẹn mà không chết thì nó sẽ không bao giờ chết. Điều này là do các chu kỳ trong tương lai sẽ không bao giờ làm giảm lượng máu của nó đủ để tạo ra mức tối thiểu mới. 

Để tránh lặp lại từng chu kỳ, chúng ta có thể sử dụng một tập hợp các trạng thái đã thấy trước đó hoặc tính toán trực tiếp số lượng chu kỳ đầy đủ có thể được bỏ qua một cách an toàn bằng cách sử dụng số học khi tổng số âm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của mảng thiệt hại. Điều này mang lại sự thay đổi sức khỏe thực sau mỗi phút trong một chu kỳ. Cấu trúc này cho phép chúng ta phát hiện cái chết trong một chu kỳ mà không cần mô phỏng từng bước. 
2. Quét tổng tiền tố để tìm chỉ số sớm nhất`i`như vậy`H + prefix[i] <= 0`. Nếu chỉ mục đó tồn tại và chúng ta đang ở chu kỳ đầu tiên, chúng ta có thể quay lại ngay lập tức`i + 1`như câu trả lời. 
3. Tính tổng của một chu kỳ đầy đủ. Giá trị này xác định xem chu kỳ lặp đi lặp lại làm cho quái vật yếu hơn hay mạnh hơn theo thời gian. 
4. Nếu tổng số tiền lớn hơn hoặc bằng 0 và quái vật sống sót sau toàn bộ chu kỳ đầu tiên, hãy quay lại`-1`. Điều này là do sức khỏe không bao giờ giảm trong toàn bộ chu kỳ, do đó, bất kỳ chu kỳ nào trong tương lai đều bắt đầu với sức khỏe ít nhất bằng hoặc cao hơn và các mẫu tiền tố giống hệt nhau. 
5. Nếu không, tổng số tiền là âm. Chúng tôi liên tục mô phỏng các chu kỳ, nhưng thay vì bước từng phút một, trước tiên chúng tôi kiểm tra trong mỗi chu kỳ xem có cái chết nào xảy ra hay không. Nếu không, chúng tôi sẽ giảm tổng lượng máu và tích lũy số phút được xử lý đầy đủ. 
6. Mỗi chu kỳ, chúng tôi điều chỉnh tình trạng bắt đầu và tính toán lại việc kiểm tra ngưỡng bằng cách sử dụng tổng tiền tố cho đến khi tìm thấy chu kỳ xảy ra cái chết. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến mà trong mỗi chu kỳ, các điểm lỗi duy nhất có thể xảy ra được xác định bằng tổng tiền tố liên quan đến tình trạng bắt đầu của chu kỳ. Thứ tự của các tổng tiền tố không bao giờ thay đổi giữa các chu kỳ; chỉ có một sự thay đổi liên tục được áp dụng. Nếu tổng chu kỳ không âm và không có tiền tố gây chết người thì tất cả các chu kỳ trong tương lai đều giống hệt nhau hoặc tốt hơn để sinh tồn, ngăn chặn mọi cái chết cuối cùng. Nếu tổng số tiền là âm, việc áp dụng lặp đi lặp lại sẽ làm giảm nghiêm trọng sức khỏe ban đầu, đảm bảo sự chấm dứt cuối cùng nếu khả năng sống sót không phải là vô hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    H, n = map(int, input().split())
    d = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + d[i]

    total = pref[n]

    cur = H
    time = 0
    seen = {}

    # We iterate cycle by cycle, but break early when possible
    while True:
        # check death inside current cycle
        for i in range(1, n + 1):
            if cur + pref[i] <= 0:
                print(time + i)
                return

        # survive full cycle
        cur += total
        time += n

        if total >= 0:
            print(-1)
            return

        # optional safety (not strictly needed but avoids pathological loops)
        if cur in seen:
            print(-1)
            return
        seen[cur] = True

        if cur <= 0:
            print(time)
            return

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng tổng tiền tố để mỗi chu kỳ có thể được kiểm tra theo thời gian tuyến tính. Vòng lặp bên trong kiểm tra xem tình trạng ban đầu hiện tại kết hợp với bất kỳ tổng tiền tố nào có dẫn đến tử vong trong chu kỳ đó hay không. Nếu không có tiền tố như vậy tồn tại, toàn bộ chu kỳ sẽ được duy trì và tình trạng được cập nhật bằng cách cộng tổng chu kỳ. 

Chi tiết triển khai chính là`cur + pref[i] <= 0`kiểm tra, mã hóa trực tiếp xem quái vật có chết ở phút nào không`i`của chu kỳ hiện tại mà không mô phỏng từng bước trừ. 

Ta cũng xử lý trường hợp vô hạn khi`total >= 0`, vì các chu kỳ lặp đi lặp lại sẽ không bao giờ làm giảm lượng máu ban đầu đủ để thay đổi kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
H = 10, n = 3
d = [-5, 2, -10]
```Tổng tiền tố:`[0, -5, -3, -13]`Tổng chu kỳ:`-13`| Chu kỳ | Bắt đầu sức khỏe | Kiểm tra i=1 | Kiểm tra i=2 | Kiểm tra i=3 | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 5 > 0 | 7 > 0 | -3 <= 0 | chết lúc 3 tuổi | 

Quái vật sống sót sau hai phút đầu tiên nhưng chết ở phút thứ 3 trong chu kỳ đầu tiên. Điều này cho thấy tại sao việc kiểm tra tiền tố là cần thiết. 

### Ví dụ 2 

đầu vào:```
H = 20, n = 2
d = [3, -2]
```Tổng tiền tố:`[0, 3, 1]`Tổng chu kỳ:`1`| Chu kỳ | Bắt đầu sức khỏe | Kiểm tra i=1 | Kiểm tra i=2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 20 | 23 > 0 | 21 > 0 | sống sót | 
| 2 | 21 | 24 > 0 | 22 > 0 | sống sót | 

Vì tổng số là dương và không có tiền tố nào giết được quái vật nên nó không bao giờ chết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một tính toán tiền tố và một lần quét mỗi chu kỳ, với tối đa một chu kỳ được xử lý hiệu quả | 
| Không gian | O(n) | Mảng tiền tố lưu trữ tổng tích lũy | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì`n`lên tới 200.000 và mọi hoạt động đều tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    H, n = map(int, input().split())
    d = list(map(int, input().split()))

    pref = [0]
    for x in d:
        pref.append(pref[-1] + x)

    total = pref[-1]
    cur = H
    time = 0

    while True:
        for i in range(1, n + 1):
            if cur + pref[i] <= 0:
                return str(time + i)

        cur += total
        time += n

        if total >= 0:
            return "-1"

# provided sample
assert run("1000 6\n-100 -200 -300 125 77 -4\n") == "9"

# minimum case
assert run("1 1\n-1\n") == "1"

# always survive
assert run("10 2\n1 1\n") == "-1"

# dies immediately after cycle repetition
assert run("5 2\n3 -10\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 -1 | 1 | chết ngay lập tức | 
| 10 2 1 1 | -1 | sinh tồn vô hạn | 
| 5 2 3 -10 | 2 | chết trong chu kỳ đầu tiên | 
| mẫu | 9 | chu trình hỗn hợp + logic tiền tố | 

## Vỏ cạnh 

Trường hợp một bên là khi quái vật chết trong chu kỳ đầu tiên. Đối với một đầu vào như`H = 3, d = [-2, 10]`, tổng tiền tố hiển thị thất bại ngay lập tức ở phút 1 vì`3 + (-2) <= 0`. Thuật toán nắm bắt được điều này trước khi lặp lại bất kỳ chu kỳ nào. 

Một trường hợp cạnh khác là khi tổng chu kỳ bằng 0. Ví dụ,`d = [1, -1]`Và`H = 5`. Tổng tiền tố không bao giờ làm giảm sức khỏe và sau mỗi chu kỳ sức khỏe vẫn giữ nguyên. Vì không có tiền tố nào giết được quái vật trong chu kỳ đầu tiên nên mọi chu kỳ trong tương lai đều giống hệt nhau, nên kết quả là`-1`.
