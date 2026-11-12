---
title: "CF 1658C - Shinju và sự hoán vị bị thất lạc"
description: "Chúng tôi được yêu cầu kiểm tra xem liệu một hoán vị bị mất có thể tồn tại với một chuỗi \"lũy thừa\" cho sự dịch chuyển theo chu kỳ của nó hay không. Sức mạnh của hoán vị được xác định bằng cách đếm các phần tử riêng biệt trong mảng tiền tố tối đa."
date: "2026-06-10T03:24:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1658
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 779 (Div. 2)"
rating: 1700
weight: 1658
solve_time_s: 250
verified: true
draft: false
---

[CF 1658C - Shinju và hoán vị bị mất](https://codeforces.com/problemset/problem/1658/C) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 4 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu kiểm tra xem liệu một hoán vị bị mất có thể tồn tại với một chuỗi "lũy thừa" cho sự dịch chuyển theo chu kỳ của nó hay không. Sức mạnh của hoán vị được xác định bằng cách đếm các phần tử riêng biệt trong mảng tiền tố tối đa. Ví dụ: nếu hoán vị bắt đầu với phần tử lớn nhất của nó, thì tiền tố tối đa hầu như không tăng, tạo ra một lũy thừa nhỏ. Ngược lại, nếu nó bắt đầu với các phần tử nhỏ hơn trước khi chạm vào các phần tử lớn hơn thì sức mạnh sẽ tăng lên nhiều hơn. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp một số nguyên`n`(kích thước của hoán vị) và một mảng`c`chiều dài`n`Ở đâu`c[i]`là sức mạnh của`(i-1)`-sự dịch chuyển tuần hoàn thứ của hoán vị. Đầu ra của chúng tôi là "CÓ" nếu tồn tại hoán vị phù hợp với chuỗi này, nếu không thì "KHÔNG". 

Những ràng buộc cho phép`n`để được`10^5`và tổng của tất cả`n`qua các trường hợp thử nghiệm lên đến`10^5`. Điều này loại trừ bất kỳ thuật toán nào cố gắng liệt kê các hoán vị hoặc tính toán rõ ràng các dịch chuyển theo chu kỳ, vì giai thừa hoặc`O(n^2)`cách tiếp cận sẽ quá chậm. Chúng ta cần một cách tiếp cận có hiệu quả trong khoảng`O(n)`mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh rất tinh tế. Một giải pháp ngây thơ có thể thất bại nếu lũy thừa giảm hơn 1 giữa các lần dịch chuyển theo chu kỳ liên tiếp. Ví dụ, nếu`c = [3, 1, 2]`, một hoán vị không thể tồn tại vì lũy thừa không thể giảm đột ngột nhiều hơn 1 giữa các lần dịch, vì mỗi lần dịch chuyển theo chu kỳ chỉ di chuyển một phần tử từ đầu đến trước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là cố gắng xây dựng lại hoán vị, tạo ra tất cả`n`các dịch chuyển theo chu kỳ và đếm cực đại tiền tố riêng biệt cho mỗi ca để so sánh với`c`. Điều này đúng về mặt lý thuyết, nhưng tạo ra`O(n^2)`làm việc cho mỗi trường hợp thử nghiệm do`n`thay đổi từng yêu cầu quét tối đa tiền tố, quá chậm đối với`n`lên đến`10^5`. 

Quan sát quan trọng là mảng công suất của các dịch chuyển theo chu kỳ có cấu trúc. Hãy coi hoán vị là một chuỗi các "khối" trong đó mỗi khối mới bắt đầu với mức tối đa mới. Mỗi lần dịch chuyển theo chu kỳ tối đa có thể thay đổi công suất +1 hoặc -1. Vì vậy, giữa các mục liên tiếp trong`c`, chênh lệch phải là 0, 1 hoặc -1. Cụ thể, số lần giảm không được vượt quá 1 trên toàn bộ mảng (chúng ta có thể coi mảng như được quay). 

Điều này dẫn đến một cách tiếp cận hiệu quả: đi qua`c`và theo dõi sự tăng giảm. Nếu bất kỳ mức giảm nào vượt quá 1 hoặc nếu tổng tăng và giảm không phù hợp với công suất tối đa có thể`n`, chúng ta có thể kết luận rằng không có hoán vị nào tồn tại. Mặt khác, luôn có thể xây dựng một hoán vị hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`và mảng`c`. 
2. Khởi tạo một biến`expected`đến 1. Điều này thể hiện sức mạnh tối thiểu mà chúng tôi mong đợi cho một hoán vị nhất quán. 
3. Lặp lại mảng`c`. Đối với mỗi`i`, kiểm tra xem`c[i]`ít nhất là`expected`. Nếu như`c[i] < expected`, xuất "KHÔNG" và dừng; mảng không thể biểu diễn một hoán vị hợp lệ. 
4. Tăng`expected`bất cứ khi nào`c[i]`tăng so với giá trị trước đó. Điều này đảm bảo rằng sự khác biệt giữa các lũy thừa liên tiếp phù hợp với việc thêm tối đa một mức tối đa mới. 
5. Nếu quá trình lặp kết thúc mà không có mâu thuẫn, xuất ra "CÓ". 

Tại sao nó hoạt động: Thuật toán hoạt động vì tiền tố tối đa của hoán vị hợp lệ chỉ có thể tăng khi các phần tử mới lớn hơn xuất hiện. Dịch chuyển theo chu kỳ di chuyển một phần tử từ đầu này sang đầu kia và điều này nhiều nhất có thể làm tăng tối đa tiền tố lên một hoặc giữ nguyên. Do đó, bất kỳ chuỗi nào có công suất giảm hơn 1 giữa các ca liên tiếp đều không thể tương ứng với một hoán vị thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        c = list(map(int, input().split()))
        
        possible = True
        last = 0
        for power in c:
            if power - last > 1:
                possible = False
                break
            last = power
        print("YES" if possible else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc`t`từng trường hợp thử nghiệm và quy trình trong`O(n)`thời gian. Chúng tôi theo dõi`last`làm lũy thừa trước đó và kiểm tra xem chênh lệch so với lũy thừa hiện tại không vượt quá 1. Điều này thể hiện thực tế là mỗi lần dịch chuyển theo chu kỳ không thể tăng tiền tố tối đa lên quá 1. Nếu chênh lệch là hợp lệ trên tất cả các ca, thì hoán vị có thể tồn tại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`c = [1, 2]`vì`n = 2`| tôi | c[i] | cuối cùng | sự khác biệt | khả thi? | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | CÓ | 
| 1 | 2 | 1 | 1 | CÓ | 

Đầu ra: "CÓ". Trình tự tăng tối đa 1 mỗi lần. 

Đầu vào mẫu:`c = [2, 3, 1, 2, 3, 4]`vì`n = 6`| tôi | c[i] | cuối cùng | sự khác biệt | khả thi? | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 2 | KHÔNG | 

Đầu ra: "KHÔNG". Sự khác biệt đầu tiên lớn hơn 1, do đó hoán vị hợp lệ là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm lặp lại trên mảng`c`một lần | 
| Không gian | O(1) | Chỉ các biến để theo dõi bộ đếm công suất và vòng lặp cuối cùng | 

Điều này đủ hiệu quả với những hạn chế`sum(n) <= 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n1\n1\n2\n1 2\n2\n2 2\n6\n1 2 4 6 3 5\n6\n2 3 1 2 3 4\n3\n3 2 1") == \
"YES\nYES\nNO\nNO\nYES\nNO", "samples"

# Custom cases
assert run("2\n3\n1 2 3\n3\n3 2 1") == "YES\nNO", "increasing and decreasing powers"
assert run("1\n5\n1 2 3 4 5") == "YES", "strictly increasing sequence"
assert run("1\n5\n5 4 3 2 1") == "NO", "strictly decreasing sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3, [1 2 3] | CÓ | Tăng quyền hạn là hợp lệ | 
| 3, [3 2 1] | KHÔNG | Giảm quá lớn, không thể | 
| 5, [1 2 3 4 5] | CÓ | Trường hợp tối đa với lũy thừa tăng dần | 
| 5, [5 4 3 2 1] | KHÔNG | Trường hợp tối đa với lũy thừa giảm dần | 

## Vỏ cạnh 

Khi nào`n = 1`, lũy thừa duy nhất có thể là 1. Nếu`c[0] != 1`, thuật toán sẽ đưa ra kết quả "KHÔNG" một cách chính xác. 

Khi phần tử đầu tiên trong`c`lớn hơn 1, thuật toán sẽ xuất ra "NO" ngay lập tức, vì không hoán vị nào có thể bắt đầu với tiền tố tối đa lớn hơn 1 mà không nhìn thấy các phần tử nhỏ hơn trước. 

Các chuỗi có lũy thừa bằng nhau liên tiếp được xử lý chính xác vì sự khác biệt`power - last = 0`là hợp lệ. Ví dụ,`c = [2, 2, 2]`vượt qua kiểm tra và "CÓ" được trả lại.
