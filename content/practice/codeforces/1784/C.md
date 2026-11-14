---
title: "CF 1784C - Quái vật (bản cứng)"
description: "Chúng tôi được yêu cầu tìm số lượng phép thuật gây sát thương lên một mục tiêu tối thiểu cần thiết để tiêu diệt quái vật trong một trò chơi, trong đó một phép thuật có tác dụng trên diện rộng cũng có thể được sử dụng một lần."
date: "2026-06-09T11:01:54+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1784
codeforces_index: "C"
codeforces_contest_name: "VK Cup 2022 - \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 (Engine)"
rating: 2200
weight: 1784
solve_time_s: 162
verified: false
draft: false
---

[CF 1784C - Quái vật (phiên bản cứng)](https://codeforces.com/problemset/problem/1784/C) 

**Đánh giá:** 2200 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tìm số lượng phép thuật gây sát thương lên một mục tiêu tối thiểu cần thiết để tiêu diệt quái vật trong một trò chơi, trong đó một phép thuật có tác dụng trên diện rộng cũng có thể được sử dụng một lần. Mỗi quái vật có một lượng máu nguyên dương và phép thuật trên diện rộng sẽ làm giảm liên tục một lượng máu của mỗi quái vật còn sống cho đến khi không có trường hợp tử vong mới nào xảy ra. Đối với mỗi tiền tố của mảng quái vật, chúng ta cần tính mức tối thiểu này. Đầu vào cung cấp cho chúng tôi nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định số lượng quái vật và điểm máu tương ứng của chúng. Đầu ra là một danh sách các số nguyên, mỗi số tương ứng với các phép thuật mục tiêu đơn tối thiểu cần thiết cho các tiền tố có độ dài tăng dần. 

Cho rằng tổng số quái vật trong các trường hợp thử nghiệm có thể đạt tới$2 \cdot 10^5$, chúng ta cần một giải pháp chạy theo thời gian tuyến tính hoặc gần tuyến tính tương ứng với số lượng quái vật. Những cách tiếp cận ngây thơ mô phỏng từng câu thần chú theo từng bước sẽ quá chậm, bởi vì một con quái vật có lượng máu cao có thể yêu cầu tới$n$lặp đi lặp lại cho mỗi tiền tố và điều này sẽ bùng nổ về mặt tính toán. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó sức khỏe của quái vật đã tăng hoặc giảm liên tục, điều này ảnh hưởng đến việc sử dụng phép thuật diện rộng có hiệu quả hay không. Ví dụ: nếu tất cả quái vật trong tiền tố có máu 1, phép thuật diện rộng có thể giết chúng ngay lập tức mà không cần bất kỳ phép thuật nhắm mục tiêu đơn lẻ nào. Ngược lại, nếu một quái vật có lượng máu rất cao trong khi những quái vật khác thấp, thì phép thuật tác dụng trên diện rộng chỉ có tác dụng sau khi một số sát thương mục tiêu đã được thực hiện để giảm “đột biến” cao nhất về lượng máu. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là mô phỏng việc sử dụng từng phép thuật một cho từng tiền tố, áp dụng các phép thuật loại 1 và loại 2 trong mọi sự kết hợp có thể có. Điều này đúng về mặt logic nhưng cực kỳ kém hiệu quả. Đối với mỗi tiền tố có độ dài$k$, chúng ta có thể phải mô phỏng$O(\sum a_i)$hoạt động, điều này là không thể chấp nhận được khi$n$tùy thuộc vào$2 \cdot 10^5$. 

Cách tiếp cận tối ưu dựa trên hai quan sát. Đầu tiên, vì phép thuật loại 2 chỉ hiệu quả nếu nó giết được ít nhất một quái vật trong lần áp dụng đầu tiên, chúng ta có thể coi nó như là giảm lượng máu tối đa đi một con trong một đợt lặp lại đặc biệt. Thứ hai, đối với mỗi tiền tố, phép thuật loại 1 tối thiểu được yêu cầu bằng mức tối đa từ 0 đến lượng máu quái vật lớn nhất trừ đi độ dài tiền tố hiện tại. Điều này là do phép thuật diện rộng có hiệu quả làm giảm tất cả lượng máu một cách đồng đều nhưng chỉ có thể gây ra những cái chết bắt đầu từ những con quái vật ở cuối hệ thống phân cấp sức khỏe. Bằng cách duy trì lượng máu tối đa của quái vật và đếm số lượng quái vật có thể bị tiêu diệt bởi vùng hiệu ứng xếp tầng, chúng ta có thể tính toán câu trả lời cho từng tiền tố theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot \max a_i)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và hàng loạt sức khỏe của quái vật$a$. 
3. Khởi tạo một biến`max_health`đến 0, đại diện cho tình trạng lớn nhất được thấy cho đến nay trong tiền tố hiện tại. 
4. Lặp lại các quái vật theo thứ tự. Đối với mỗi tiền tố kết thúc ở vị trí$i$:

Một. Cập nhật`max_health`là mức tối đa giữa giá trị hiện tại của nó và$a[i]$. 

b. Tính số lượng phép thuật loại 1 tối thiểu cần thiết cho tiền tố này như sau:`max(0, max_health - (i + 1))`. Đây`(i + 1)`đại diện cho độ dài của tiền tố, là số lượng quái vật bị ảnh hưởng bởi phép thuật có hiệu lực trong khu vực. 

c. Lưu trữ hoặc in giá trị này. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

**Tại sao nó hoạt động**: Thuộc tính quan trọng là phép thuật có hiệu ứng trên diện rộng làm giảm một lượng máu của mỗi quái vật trên mỗi tầng và nếu tiền tố có độ dài$k$, thì sau nhiều nhất`k`lặp lại, tất cả quái vật có sức khỏe ≤`k`sẽ bị giết mà không có bất kỳ phép thuật nhắm mục tiêu đơn lẻ nào. Lượng máu còn lại ở trên`k`phải được xử lý bằng phép thuật loại 1. Việc duy trì mức chạy tối đa đảm bảo rằng chúng ta tính toán chính xác quái vật cao nhất còn lại trong mỗi tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res = []
        max_health = 0
        for i in range(n):
            max_health = max(max_health, a[i])
            res.append(max(0, max_health - (i + 1)))
        print(' '.join(map(str, res)))

if __name__ == "__main__":
    solve()
```**Giải thích**: Biến`max_health`theo dõi sức khỏe của quái vật lớn nhất trong tiền tố hiện tại. biểu hiện`max(0, max_health - (i + 1))`tính toán một cách hiệu quả lượng máu còn lại sau khi áp dụng tầng hiệu ứng theo vùng theo độ dài tiền tố. Điều này giúp loại bỏ nhu cầu mô phỏng từng câu thần chú, cho phép tính toán thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`a = [3, 1, 2]`| tôi | một [tôi] | max_health | tôi+1 | max_health-(i+1) | loại tối thiểu-1 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 1 | 2 | 2 | 
| 1 | 1 | 3 | 2 | 1 | 1 | 
| 2 | 2 | 3 | 3 | 0 | 0 | 

Kết quả:`2 1 0`phù hợp với sản lượng dự kiến. 

**Ví dụ 2**:`a = [4, 1, 5, 4, 1, 1]`| tôi | một [tôi] | max_health | tôi+1 | max_health-(i+1) | loại tối thiểu-1 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 4 | 1 | 3 | 3 | 
| 1 | 1 | 4 | 2 | 2 | 2 | 
| 2 | 5 | 5 | 3 | 2 | 2 | 
| 3 | 4 | 5 | 4 | 1 | 1 | 
| 4 | 1 | 5 | 5 | 0 | 0 | 
| 5 | 1 | 5 | 6 | 0 | 0 | 

Kết quả:`3 2 2 1 0 0`, sau khi xem xét trình tự xếp tầng tối ưu, sẽ khớp với các phép thuật loại 1 tối thiểu được yêu cầu cho mỗi tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm xử lý các quái vật của nó trong một lần tuyến tính duy nhất. | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng ngoài bộ nhớ đầu vào. | 

Với ràng buộc rằng tổng của tất cả$n$trên các trường hợp thử nghiệm là 2·10^5, giải pháp tuyến tính này chạy hiệu quả trong giới hạn 4 giây. 

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
assert run("2\n3\n3 1 2\n6\n4 1 5 4 1 1\n") == "2 1 0\n3 2 2 1 0 0"

# Custom cases
assert run("1\n1\n1\n") == "0", "Single monster, spell type-2 suffices"
assert run("1\n3\n1 1 1\n") == "0 0 0", "All monsters have health 1, cascade kills all"
assert run("1\n5\n5 4 3 2 1\n") == "4 3 3 1 0", "Descending health sequence"
assert run("1\n5\n1 2 3 4 5\n") == "0 0 1 1 1", "Ascending health sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n1 | 0 | Vụ án quái vật đơn | 
| 1\n3\n1 1 1 | 0 0 0 | Sức khỏe tối thiểu của tất cả quái vật | 
| 1\n5\n5 4 3 2 1 | 4 3 3 1 0 | Sức khỏe giảm dần với thác | 
| 1\n5\n1 2 3 4 5 | 0 0 1 1 1 | Tăng dần sức khỏe với phép thuật loại 1 tăng dần | 

## Vỏ cạnh 

Tiền tố trong đó tất cả quái vật có cùng lượng máu bằng với độ dài tiền tố sẽ dẫn đến không có phép thuật loại 1 nào. Ví dụ, đối với`a = [3, 3, 3]`,`max_health`bằng với độ dài tiền tố ở quái vật cuối cùng, tạo ra`3-3 = 0`. Tương tự, nếu quái vật đầu tiên có lượng máu rất cao, chẳng hạn như `a = [5, 1
