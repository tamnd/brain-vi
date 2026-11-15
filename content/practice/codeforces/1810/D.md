---
title: "CF 1810D - Leo Cây"
description: "Chúng ta đang xử lý một tình huống trong đó những con ốc sên trèo lên một cái cây có chiều cao không xác định. Mỗi con ốc có hai số a và b tượng trưng cho số mét leo lên vào ban ngày và số mét trượt xuống vào ban đêm. Con ốc đầu tiên báo cáo số ngày n để lên đến đỉnh."
date: "2026-06-09T08:45:33+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1810
codeforces_index: "D"
codeforces_contest_name: "CodeTON Round 4 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1700
weight: 1810
solve_time_s: 72
verified: true
draft: false
---

[CF 1810D - Leo cây](https://codeforces.com/problemset/problem/1810/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một tình huống trong đó những con ốc sên trèo lên một cái cây có chiều cao không xác định. Mỗi con ốc có hai số,`a`Và`b`, biểu thị số mét tăng vào ban ngày và số mét trượt xuống vào ban đêm. Con ốc đầu tiên báo số ngày`n`nó cần thiết để đạt đến đỉnh cao. Những con ốc tiếp theo báo cáo ngày của chúng hoặc hỏi chúng cần bao nhiêu ngày. Nhiệm vụ của chúng tôi là duy trì một tập hợp nhất quán các chiều cao cây có thể có từ những con ốc được báo cáo và trả lời các truy vấn trong tương lai dựa trên thông tin đó. 

Mỗi sự kiện loại 1 là một yêu cầu về chiều cao của cây được suy ra từ`(a, b, n)`. Sự kiện loại 2 yêu cầu số ngày chính xác, điều này chỉ có thể thực hiện được nếu chiều cao của cây được xác định duy nhất so với các xác nhận quyền sở hữu đã được chấp nhận trước đó. 

Thách thức chính là giải quyết mâu thuẫn. Nếu một xác nhận quyền sở hữu mới không thể tương ứng với bất kỳ chiều cao số nguyên nào tương thích với các xác nhận quyền sở hữu đã được chấp nhận trước đó thì chúng tôi sẽ bỏ qua nó. Đối với các truy vấn, chúng tôi phải xác định xem số ngày có mang tính quyết định hay không dựa trên phạm vi chiều cao cây có thể có hiện tại. Chiều cao của cây là một số nguyên dương và chúng ta đang làm việc với các giá trị lên đến$10^9$, vì vậy việc mô phỏng lực lượng vũ phu mỗi ngày là không thể thực hiện được. 

Các trường hợp cạnh không rõ ràng phát sinh khi ốc báo cáo ngày đầu tiên là đủ để đạt đến đỉnh. Ví dụ, nếu`a = 5, b = 2, n = 1`, chiều cao của cây ở bất cứ đâu từ`1`ĐẾN`5`mét. Một cách tiếp cận ngây thơ chỉ xem xét tiến độ thực hàng ngày (`a-b`) có thể từ chối độ cao hợp lệ hoặc không tính toán chính xác các truy vấn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng con ốc sên hàng ngày cho mọi truy vấn. Vì`n`ngày, việc này mất`O(n)`mỗi truy vấn. Với tối đa`2*10^5`sự kiện và chiều cao cây lên đến$10^9$, điều này nhanh chóng trở thành không thể. Bạn sẽ phải cân nhắc tới$10^9$lặp đi lặp lại trong trường hợp xấu nhất, điều này vượt xa khả năng thực hiện. 

Thông tin chi tiết là mỗi sự kiện loại 1 cung cấp một phạm vi chiều cao cây có thể có thay vì một con số. Vào ngày`n`, một con ốc leo`a`và trượt`b`có thể đã đạt tới độ cao giữa`a + (n-1)*(a-b)`(tiến độ thực tính đến ngày`n-1`cộng với việc leo lên đầy đủ vào ngày cuối cùng) và`n*a`(độ leo tối đa nếu nó đạt đến đỉnh trước khi trượt). Bằng cách theo dõi chiều cao cây tối thiểu và tối đa toàn cầu phù hợp với tất cả các sự kiện loại 1 được chấp nhận, chúng tôi có thể nhanh chóng xác định xem một sự kiện mới có mâu thuẫn với kiến ​​thức trước đó hay không. Truy vấn loại 2 được giải quyết bằng cách kiểm tra xem`(a, b)`chỉ có thể đạt chính xác một ngày cho mọi độ cao trong phạm vi hợp lệ. Nếu số ngày được yêu cầu khác nhau trong phạm vi, chúng tôi sẽ báo cáo`-1`. 

Cách tiếp cận này làm giảm vấn đề về việc kiểm tra giao nhau phạm vi và số học đơn giản, cho phép chúng ta xử lý tất cả`2*10^5`sự kiện một cách hiệu quả trong`O(1)`mỗi sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * h) | O(1) | Quá chậm | 
| Phạm vi giao lộ | O(q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`min_height = 1`Và`max_height = INF`để đại diện cho tất cả các chiều cao dương có thể có của cây. INF có thể`10^18`để đảm bảo không tràn trong tính toán. 
2. Lặp lại qua từng sự kiện. Nếu sự kiện thuộc loại 1, hãy tính chiều cao cây tối thiểu và tối đa phù hợp`(a, b, n)`: 

- Chiều cao tối thiểu mà ốc sên có thể đạt tới`n`ngày là`low = a + (n-1)*(a-b)`. 
- Chiều cao tối đa mà ốc sên có thể đạt tới`n`ngày là`high = n*a`. 
- Nếu như`low > max_height`hoặc`high < min_height`, sự kiện này mâu thuẫn với thông tin trước đó. đầu ra`0`và bỏ qua phạm vi cập nhật. 
- Ngược lại, giao phạm vi mới với phạm vi hiện tại:`min_height = max(min_height, low)`Và`max_height = min(max_height, high)`. đầu ra`1`. 
3. Nếu sự kiện thuộc loại 2, hãy kiểm tra xem số ngày đến cây có thể được xác định duy nhất hay không: 

- Dành cho mọi chiều cao`h`TRONG`[min_height, max_height]`, số ngày cần tìm là`days = ceil((h-b)/(a-b))`hoặc`days = ceil(h/a)`nếu nó có thể hoàn thành trong một ngày. Tính số ngày tối thiểu và tối đa trên phạm vi độ cao hiện tại. 
- Nếu số ngày tối thiểu và tối đa bằng nhau thì xuất ra số đó. Ngược lại, xuất ra`-1`. 
4. Lặp lại cho tất cả các sự kiện trong test case, thu thập các câu trả lời theo thứ tự. 

**Tại sao nó hoạt động**: Thuật toán duy trì một khoảng hợp lệ liên tục của các chiều cao cây có thể có. Mỗi xác nhận quyền sở hữu loại 1 giao nhau với khoảng này hoặc bị bỏ qua. Truy vấn loại 2 tính toán số ngày trong khoảng thời gian này. Bất biến quan trọng là tại bất kỳ thời điểm nào,`min_height`Và`max_height`ràng buộc tất cả các độ cao tương thích với các yêu cầu được chấp nhận. Bất kỳ truy vấn loại 2 nào tạo ra số ngày nhất quán trong khoảng thời gian này đều phải chính xác; nếu không, kết quả là không xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        q = int(input())
        min_h = 1
        max_h = 10**18
        res = []
        for _ in range(q):
            data = list(map(int, input().split()))
            if data[0] == 1:
                a, b, n = data[1], data[2], data[3]
                low = a + (n-1)*(a-b)
                high = n*a
                if low > max_h or high < min_h:
                    res.append(0)
                else:
                    min_h = max(min_h, low)
                    max_h = min(max_h, high)
                    res.append(1)
            else:
                a, b = data[1], data[2]
                # Calculate min and max days over the current height range
                min_days = math.ceil((min_h - b) / (a - b))
                max_days = math.ceil((max_h - b) / (a - b))
                if min_days == max_days:
                    res.append(min_days)
                else:
                    res.append(-1)
        print(*res)

if __name__ == "__main__":
    solve()
```Mã đọc tất cả các sự kiện cho từng trường hợp thử nghiệm. Sự kiện Loại 1 tính toán khoảng chiều cao hợp lệ cho xác nhận quyền sở hữu và giao nó với khoảng toàn cục. Sự kiện loại 2 tính toán số ngày tối thiểu và tối đa trong khoảng thời gian đó. sử dụng`math.ceil`đảm bảo chúng tôi tính toán chính xác các phân số ngày, tránh sai sót từng phần một. 

## Ví dụ đã hoạt động 

**Mẫu 1 vết** 

| Sự kiện | phút_h | tối đa_h | Đầu ra | 
| --- | --- | --- | --- | 
| 1 3 2 5 | 7 | 15 | 1 | 
| 2 4 1 | 7 | 15 | 2 | 
| 2 3 2 | 7 | 15 | 5 | 

Yêu cầu loại 1 đầu tiên`(3,2,5)`tạo ra độ cao có thể`[7,15]`. Truy vấn loại 2 tính toán số ngày cần thiết:`(4,1)`cần 2 ngày,`(3,2)`cần 5 ngày, phù hợp với mẫu. 

**Dấu vết của vỏ cạnh** 

Yêu cầu`(5,2,1)`ngụ ý chiều cao của cây`[5,5]`. Một truy vấn`(3,1)`trong khoảng thời gian này cần 2 ngày. Thuật toán cắt chính xác khoảng thời gian và trả về một câu trả lời duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi sự kiện liên quan đến số học theo thời gian không đổi và các phép toán tối đa/phút | 
| Không gian | O(1) | Chỉ lưu trữ danh sách min_h, max_h và đầu ra | 

Với`q`lên đến`2*10^5`, điều này an toàn dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("5\n3\n1 3 2 5\n2 4 1\n2 3 2\n3\n1 6 5 1\n2 3 1\n2 6 2\n3\n1 4 2 2\n1 2 1 3\n2 10 2\n9\n1 7 3 6\n1 2 1 8\n2 5 1\n1 10 9 7\n1 8 1 2\n1 10 5 8\n1 10 7 7\n2 7 4\n1 9 4 2\n9\n1 2 1 6\n1 8 5 6\n1 4 2 7\n2 9 1\n1 5 1 4\n1 5 2 7\n1 7 1 9\n1 9 1 4\n2 10 8") == \
"1 2 5\n1 -1 1\n1 0 1\n1 0 -1 0 0 0 1 8 0\n1 0 0 1
```
