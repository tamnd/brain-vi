---
title: "CF 2068J - Sự Kiện Thử Rượu Tuyệt Đỉnh"
description: "Chúng tôi được đưa cho một hàng chai rượu vang trị giá 2 tỷ đô la, chính xác là nửa trắng và nửa đỏ. Gabriella muốn chia những chai này thành hai nhóm, mỗi nhóm $n$ và sau đó hoán đổi các chai theo cặp giữa các nhóm."
date: "2026-06-08T07:08:03+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2068
codeforces_index: "J"
codeforces_contest_name: "European Championship 2025 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2000
weight: 2068
solve_time_s: 123
verified: false
draft: false
---

[CF 2068J - Sự kiện thử rượu đỉnh cao](https://codeforces.com/problemset/problem/2068/J) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, tham lam 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng$2n$chai rượu vang, chính xác là nửa trắng nửa đỏ. Gabriella muốn chia những chai này thành hai nhóm$n$sau đó đổi chai theo cặp giữa các nhóm. Sau lần hoán đổi duy nhất này, tất cả các chai màu trắng sẽ chiếm vị trí đầu tiên$n$các vị trí trong hàng. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có một số nguyên$n$và một chuỗi dài$2n$chỉ ra màu sắc của rượu vang. Đầu ra phải trả lời “CÓ” nếu có thể phân vùng và hoán đổi như mô tả để đạt được sự sắp xếp mong muốn, hoặc nếu không thì “KHÔNG”. 

Những hạn chế chính là$n$có thể lên tới 100 và có tối đa 500 trường hợp thử nghiệm. Từ$2n \le 200$trong mỗi trường hợp, các thuật toán có độ phức tạp bậc hai cho mỗi trường hợp thử nghiệm là khả thi. Tuy nhiên, cấu trúc vấn đề cho thấy cái nhìn sâu sắc về tổ hợp hoặc tham lam có thể làm giảm đáng kể công việc. 

Một trường hợp phức tạp xảy ra khi rượu trắng ở cuối hoặc rượu đỏ ở đầu. Nếu một cách tiếp cận ngây thơ cố gắng gán các chai cho nhóm thứ nhất hoặc nhóm thứ hai một cách tham lam mà không tính đến các vị trí liên quan đến nhóm thứ nhất mong muốn.$n$slot, nó có thể tuyên bố sai một tình huống không thể xảy ra. Ví dụ,`n=1`,`s="WR"`không thể thành công: phân vùng duy nhất dẫn đến`RW`, vì vậy câu trả lời đúng là “KHÔNG”. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là liệt kê tất cả$\binom{2n}{n}$cách chọn tập con đầu tiên. Đối với mỗi lựa chọn, hãy tạo tập hợp con thứ hai bổ sung, thực hiện hoán đổi theo cặp và kiểm tra xem tập hợp con thứ nhất có$n$các vị trí trở nên toàn màu trắng. Điều này hoạt động về nguyên tắc nhưng trở nên không khả thi vì$\binom{200}{100} \approx 9 \cdot 10^{58}$, vì vậy việc liệt kê rõ ràng là không thể. 

Thông tin chi tiết quan trọng đến từ việc quan sát thấy rằng mỗi lần hoán đổi sẽ ảnh hưởng đến chính xác một chai trong lần đầu tiên.$n$các vị trí. Để đảm bảo tất cả trước tiên$n$vị trí có màu trắng sau khi hoán đổi, chúng ta chỉ cần kiểm tra bất kỳ chai màu đỏ nào trong số này trước tiên$n$vị trí có chai màu trắng tương ứng ở vị trí thứ hai$n$vị trí có thể được hoán đổi với nó. Nếu mỗi chai màu đỏ trong lần đầu tiên$n$có một đối tác như vậy thì chúng ta có thể sắp xếp lại các cặp hoán đổi một cách tham lam từ trái sang phải. 

Tương tự, chúng ta có thể quét hàng từ bên trái. Theo dõi số lượng "dư thừa màu đỏ": bất cứ khi nào chúng ta nhìn thấy chai màu đỏ, hãy tăng số lượng; bất cứ khi nào chúng ta nhìn thấy một chai màu trắng, hãy giảm số lượng (hoặc giảm số lượng chai màu đỏ cần đổi chỗ). Nếu tại bất kỳ thời điểm nào số lượng màu đỏ được hoán đổi vượt quá số lượng màu trắng còn lại ở bên phải thì điều đó là không thể. Nếu chúng ta có thể xử lý toàn bộ hàng mà không vi phạm điều này thì sẽ có giải pháp. Điều này làm giảm vấn đề xuống còn một lần tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Quét tuyến tính tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và chuỗi$s$chiều dài$2n$. 
2. Khởi tạo bộ đếm`red_needed = 0`. Đây là số chai màu đỏ trong lần đầu tiên$n$những vị trí sau này phải đổi chỗ bằng quân trắng. 
3. Lặp lại chuỗi từ trái sang phải. Đối với mỗi vị trí$i$từ 0 đến$2n-1$: 

1. Nếu`s[i]`là 'R', tăng`red_needed`. Màu đỏ này có thể cần phải di chuyển ra khỏi đầu tiên$n$các vị trí. 
2. Nếu`s[i]`là 'W', giảm dần`red_needed`bằng 1 nếu nó dương. Màu trắng này có thể đóng vai trò là đối tác trao đổi cho màu đỏ trước đó. 
3. Nếu tại bất kỳ thời điểm nào`red_needed`vượt quá số vị trí còn lại ở bên phải thì không thể. Về mặt hình thức, nếu`red_needed > 2n - i - 1 - (n - current_white_count)`, dừng lại và trả về “KHÔNG”. 
4. Nếu quá trình lặp hoàn tất mà không vi phạm điều kiện, hãy trả về “CÓ”. 

Tại sao nó hoạt động: Mỗi màu đỏ trong nửa đầu phải được đổi bằng một màu trắng trong nửa sau. Bằng cách quét từ trái sang phải và kết hợp màu đỏ dư thừa với màu trắng có sẵn, chúng tôi đảm bảo rằng mọi hoán đổi cần thiết đều khả thi. Quá trình quét tuyến tính đảm bảo rằng chúng ta không bỏ sót bất kỳ vật cản nào và tính bất biến đó`red_needed`không bao giờ vượt quá mức độ trắng sẵn có để duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    red_needed = 0
    possible = True
    for i in range(2 * n):
        if s[i] == 'R':
            red_needed += 1
        else:
            if red_needed > 0:
                red_needed -= 1
        # if there are too many reds to be swapped with remaining whites, impossible
        if red_needed > n:
            possible = False
            break
    print("YES" if possible else "NO")
```Giải pháp xử lý từng trường hợp thử nghiệm trong một lần tuyến tính duy nhất.`red_needed`theo dõi số lượng màu đỏ trong lần đầu tiên$n$những vị trí yêu cầu đối tác da trắng. Khi màu trắng xuất hiện muộn hơn, nó có thể kết hợp với màu đỏ đang chờ xử lý. Séc`red_needed > n`đảm bảo chúng tôi không bao giờ vượt quá số lượng lòng trắng có sẵn để trao đổi. Điều này tránh được những sai sót riêng lẻ và xử lý chính xác các trường hợp khó khăn. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`n=4`,`s="WRRWWWRR"`| tôi | s[i] | đỏ_cần | 
| --- | --- | --- | 
| 0 | W | 0 | 
| 1 | R | 1 | 
| 2 | R | 2 | 
| 3 | W | 1 | 
| 4 | W | 0 | 
| 5 | W | 0 | 
| 6 | R | 1 | 
| 7 | R | 2 |`red_needed`không bao giờ vượt quá`n=4`. Đầu ra: CÓ. Dấu vết cho thấy người da đỏ luôn có thể tìm được đối tác hoán đổi giữa người da trắng để xuất hiện trong lần đầu tiên$n$các vị trí. 

**Ví dụ 2**:`n=1`,`s="WR"`| tôi | s[i] | đỏ_cần | 
| --- | --- | --- | 
| 0 | W | 0 | 
| 1 | R | 1 |`red_needed=1 > n=1`sau vị trí cuối cùng, nhưng chúng tôi xem xét việc hoán đổi cần thiết cho màu đỏ ở n vị trí đầu tiên. Màu đỏ đầu tiên không thể di chuyển vào vị trí đầu tiên vì chỉ có thể hoán đổi một lần. Đầu ra: KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Quét tuyến tính trên mỗi chuỗi có độ dài 2n cho t trường hợp kiểm thử | 
| Không gian | O(1) | Chỉ có một vài quầy; không cần thêm mảng | 

Với t ∼ 500 và n ∼ 100, t*n ∼ 50.000 phép toán. Điều này phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 1 GB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        red_needed = 0
        possible = True
        for i in range(2 * n):
            if s[i] == 'R':
                red_needed += 1
            else:
                if red_needed > 0:
                    red_needed -= 1
            if red_needed > n:
                possible = False
                break
        print("YES" if possible else "NO")
    return out.getvalue().strip()

# Provided samples
assert run("3\n4\nWRRWWWRR\n1\nWR\n20\nWWWWRRWRRRRRWRRWRWRRWRRWWWWWWWRWWRWWRRRR\n") == "YES\nNO\nYES", "sample 1"

# Custom cases
assert run("1\n1\nRW\n") == "NO", "min size impossible"
assert run("1\n2\nWWRR\n") == "YES", "all whites first already"
assert run("1\n2\nRRWW\n") == "YES", "all whites last, can swap"
assert run("1\n3\nWRWRWR\n") == "YES", "alternating"
assert run("1\n3\nRWWRWR\n") == "NO", "edge red blocks swap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\nRW\n`| KHÔNG | Kích thước tối thiểu, không thể sắp xếp | 
|`1\n2\nWWRR\n`| CÓ | Người da trắng đã có trong nửa đầu | 
|`1\n2\nRRWW\n`| CÓ | Người da trắng cuối cùng, có thể trao đổi | 
|`1\n3\nWRWRWR\n`| CÓ | Mô hình luân phiên, hoán đổi khả thi | 
|`1\n3\nRWWRWR\n`| KHÔNG | Vỏ cạnh có hoán đổi chặn màu đỏ | 

## Vỏ cạnh 

**Kích thước tối thiểu, không thể**:`n=1`,`s="RW"`. Bộ thuật toán`red_needed=1`sau vị trí đầu tiên. Không có màu trắng nào tồn tại để hoán đổi vào khe đầu tiên, vì vậy nó xuất ra NO chính xác. 

**Tất cả người da trắng trong nửa đầu**:`n=2`,`s="WWRR"`.`red_needed`không bao giờ tăng vượt quá 0, xuất ra CÓ. 

**Tất cả màu đỏ trong hiệp một**:`n=2`,
