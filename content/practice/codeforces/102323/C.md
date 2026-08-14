---
title: "CF 102323C - Ếch Nhảy"
description: "Chúng ta có một đấu trường hình tròn với (N) vị trí được đánh số từ (0) đến (N-1). Mọi vị trí đều có thể là một tảng đá, viết là R, hoặc một cái ao, viết là P. Con ếch có thể bắt đầu ở bất kỳ tảng đá nào. Sau khi chọn độ dài bước nhảy (K), mỗi lần nhảy sẽ di chuyển con ếch từ vị trí (i) đến [ (i+K)bmod N."
date: "2026-08-14T00:54:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "C"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 827
verified: true
draft: false
---

[CF 102323C - Ếch nhảy](https://codeforces.com/problemset/problem/102323/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13 phút 47 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đấu trường hình tròn với (N) vị trí được đánh số từ (0) đến (N-1). Mọi vị trí đều là một tảng đá, được viết là`R`, hoặc một cái ao, được viết là`P`. Con ếch có thể bắt đầu ở bất kỳ tảng đá nào. 

Sau khi chọn độ dài bước nhảy (K), mỗi lần nhảy sẽ di chuyển con ếch từ vị trí (i) sang 

[ 
(i+K)\bmod N. 
] 

Con ếch tiếp tục thực hiện cú nhảy này cho đến khi nó trở lại vị trí ban đầu. Độ dài bước nhảy là hợp lệ nếu tồn tại ít nhất một tảng đá xuất phát sao cho mọi vị trí đã ghé thăm trước khi quay lại cũng là một tảng đá. Chúng ta cần đếm có bao nhiêu (K) khác biệt trong phạm vi (1) đến (N-1) là hợp lệ. Các ràng buộc chính thức có (3\le N\le10^5), với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Kích thước (N=10^5) ngay lập tức loại trừ các thuật toán kiểm tra mọi sự kết hợp giữa độ dài bước nhảy, vị trí bắt đầu và vị trí đã truy cập. Giới hạn trên bậc ba sẽ có khoảng (10^{15}) hoạt động. Chúng ta cần khai thác cấu trúc số học của việc cộng nhiều lần cùng một (K) modulo (N). 

Có một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Ví dụ: nếu mọi vị trí đều là một tảng đá`RRR`, cả (K=1) và (K=2) đều hoạt động, vì vậy câu trả lời là`2`. Một giải pháp chỉ kiểm tra một độ dài bước nhảy cụ thể sẽ bỏ sót thực tế là mọi độ dài khác 0 đều hoạt động. 

Ví dụ: nếu mọi vị trí đều là một cái ao`PPP`, câu trả lời là`0`. Hoàn toàn không có vị trí xuất phát hợp pháp nào cả, do đó, một giải pháp giả định vị trí (0) là một hòn đá xuất phát hợp lệ có thể thất bại trước khi nó kiểm tra các bước nhảy. 

Điểm bắt đầu được phép là bất kỳ tảng đá nào, không nhất thiết phải ở vị trí (0). Vì`PRRR`, (K=2) hoạt động bằng cách bắt đầu từ vị trí (1): con ếch ghé thăm vị trí (1) và (3), cả hai hòn đá và quay trở lại (1). Như vậy đáp án đúng là`1`. Việc triển khai luôn bắt đầu từ vị trí (0) sẽ từ chối không chính xác (K=2). 

Cuối cùng, bản thân độ dài bước nhảy không giống với số lượng vị trí riêng biệt được truy cập. Vì`RRPR`, (K=2) thăm các vị trí (0,2,0,\ldots) nên hợp lệ, trong khi (K=1) và (K=3) thăm mọi vị trí và gặp ao ở vị trí (2). Câu trả lời đúng là`1`. Sự khác biệt đến từ ước chung lớn nhất của (K) và (N). 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp nhất là thử mọi (K), thử mọi hòn đá có thể bắt đầu và mô phỏng các bước nhảy cho đến khi con ếch quay trở lại điểm xuất phát hoặc đáp xuống ao. Mỗi mô phỏng có thể kiểm tra tối đa (N) vị trí, do đó việc triển khai theo nghĩa đen có giới hạn trên (O(N^3)). Với (N=10^5), điều đó có nghĩa là có tới (10^{15}) lượt kiểm tra vị trí ứng viên, vượt xa thời gian có sẵn. 

Lực lượng vũ phu hoạt động vì nó tuân theo chính xác những gì con ếch làm, nhưng về cơ bản nó lặp lại cùng một cấu trúc tuần hoàn nhiều lần. Quan sát quan trọng là việc thêm (K) modulo (N) liên tục không tạo ra một tập hợp con các vị trí tùy ý. Cấu trúc của nó hoàn toàn được xác định bởi 

[ 
g=\gcd(K,N). 
] 

Bắt đầu từ (các) vị trí, sau (t) nhảy con ếch ở vị trí 

[ 
s+tK\pmod N. 
] 

Mọi giá trị của (tK\pmod N) là bội số của (g) và tất cả bội số của (g) xảy ra trước khi chuỗi quay về (s). Do đó, con ếch ghé thăm chính xác các vị trí đồng dạng với (s) modulo (g). 

Điều này thay đổi vấn đề một cách đáng kể. Thay vì kiểm tra từng (K) riêng biệt, chúng ta có thể hỏi liệu ước số (g) của (N) có ít nhất một lớp dư lượng modulo (g) chỉ chứa đá hay không. Nếu một lớp như vậy tồn tại thì mọi độ dài bước nhảy có ước số chung lớn nhất với (N) là (g) là hợp lệ. 

Đối với ước số cố định (g), chúng ta chỉ cần kiểm tra phần dư modulo (g) nào chứa ao. Nếu một số cặn không bao giờ xuất hiện giữa các vị trí trong ao thì mọi vị trí trong lớp cặn đó đều là đá, vì vậy việc chọn bất kỳ loại đá nào từ lớp đó sẽ mang lại một điểm khởi đầu hợp lệ. 

Chỉ có một số lượng nhỏ ước số của một số lên tới (10^5). Chúng ta liệt kê các ước của (N), kiểm tra từng ước và cuối cùng phân loại mọi (K) theo (\gcd(K,N)). Với tối đa khoảng 128 ước số cho phạm vi (N) này, công việc kiểu (O(N\tau(N))) có thể dễ dàng quản lý được, trong đó (\tau(N)) là số lượng ước số của (N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\tau(N)+N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi hình tròn và ghi lại chỉ số của tất cả các vị trí trong ao. Chúng ta chỉ cần vị trí ao vì lớp cặn có giá trị chính xác khi nó không chứa ao. 
2. Tạo mọi ước số (g) của (N). Chỉ các ước số mới có thể xuất hiện dưới dạng (\gcd(K,N)), do đó việc kiểm tra các ước số không phải là ước số sẽ làm những công việc không cần thiết. 
3. Với mỗi ước số thực sự (g<N), hãy xem xét tất cả các vị trí ao theo modulo (g). Đánh dấu phần dư (r) nếu có ao ở vị trí nào đó phù hợp với (r\pmod g). 
4. Nếu ít nhất một dư lượng modulo (g) vẫn chưa được đánh dấu thì tuyên bố (g) hợp lệ. Phần cặn đó không hề có ao, nên mọi vị trí trong đó đều là đá và con ếch có thể bắt đầu từ đó. 
5. Nếu có ít hơn (g) vị trí ao thì tuyên bố ngay (g) hợp lệ. Có (g) loại dư lượng nhưng ít hơn (g) ao, vì vậy các ao không thể chiếm hết mọi loại dư lượng. 
6. Với mỗi bước nhảy (K) từ (1) đến (N-1), hãy tính (g=\gcd(K,N)). Nếu (g) ​​được đánh dấu hợp lệ, hãy thêm một vào câu trả lời. 

Lý do bước 6 là đủ là bất biến trung tâm của nghiệm: tất cả các độ dài bước nhảy có cùng giá trị (\gcd(K,N)) đều truy cập chính xác cùng một loại lớp dư lượng. Giá trị số thực tế của chúng không thành vấn đề khi gcd của chúng với (N) được cố định. 

### Tại sao nó hoạt động 

Đặt (g=\gcd(K,N)) và đặt vị trí bắt đầu là (s). Sau khi (t) nhảy, con ếch chiếm giữ (s+tK\pmod N). Vì (K=gK') và (N=gN'), trong đó (K') và (N') là nguyên tố cùng nhau, nên dãy (tK'\pmod{N'}) truy cập vào mọi dư lượng modulo (N'). Nhân với (g) có nghĩa là vị trí ban đầu mà con ếch đã ghé thăm chính xác là 

[ 
s,\ s+g,\ s+2g,\ldots 
] 

xung quanh vòng tròn.

Do đó, con ếch có thể hoàn thành buổi thực hành một cách chính xác khi có ít nhất một lớp cặn modulo (g) bao gồm toàn bộ đá. Kiểm tra ước số của chúng tôi kiểm tra chính xác điều kiện đó và phép tính gcd cuối cùng sẽ gán mọi (K) có thể có cho lớp chính xác. Do đó, mọi độ dài bước nhảy được tính đều hợp lệ và mọi độ dài bước nhảy hợp lệ đều được tính. 

## Giải pháp Python```python
import sys
from math import gcd, isqrt

input = sys.stdin.readline

def solve_string(s: str) -> int:
    n = len(s)

    ponds = [i for i, ch in enumerate(s) if ch == 'P']

    # If there are no ponds, every non-zero jump length works.
    if not ponds:
        return n - 1

    # Generate all divisors of n.
    divisors = []
    for d in range(1, isqrt(n) + 1):
        if n % d == 0:
            divisors.append(d)
            if d * d != n:
                divisors.append(n // d)

    can_jump = [False] * (n + 1)
    pond_count = len(ponds)

    for g in divisors:
        # gcd(K, n) can never equal n for 1 <= K < n.
        if g == n:
            continue

        # With fewer ponds than residue classes, some residue is pond-free.
        if pond_count < g:
            can_jump[g] = True
            continue

        seen = bytearray(g)
        covered = 0

        for p in ponds:
            r = p % g
            if not seen[r]:
                seen[r] = 1
                covered += 1

                # Every residue contains a pond, so no all-rock class exists.
                if covered == g:
                    break

        can_jump[g] = covered < g

    answer = 0

    for k in range(1, n):
        if can_jump[gcd(k, n)]:
            answer += 1

    return answer

def main():
    s = input().strip()
    print(solve_string(s))

if __name__ == "__main__":
    main()
```các`ponds`mảng lưu trữ chính xác các vị trí có thể làm mất hiệu lực của lớp dư lượng. Modulo dư lượng (g) là tốt nếu không có ao chứa nào có dư lượng đó. 

Việc tạo số chia chỉ lên tới (\sqrt N). Khi`d`chia rẽ`n`, cả hai`d`Và`n // d`là các ước số, trừ khi chúng bằng nhau. Ước số (N) được bộ tạo giữ lại nhưng bị bỏ qua sau đó vì không có (K) nào trong phạm vi yêu cầu có (\gcd(K,N)=N). 

các`pond_count < g`phím tắt rất hữu ích cả về mặt khái niệm và thực tế. Với số lượng ao ít hơn các loại cặn, không thể nào mỗi lớp cặn lại có một ao, vì vậy ít nhất một lớp phải chỉ chứa đá. 

các`bytearray`được sử dụng như một mảng nhỏ các cờ Boolean.`seen[r]`ghi lại liệu một ao có xuất hiện trong lớp dư lượng (r) hay không. Ngay khi tất cả (g) dư lượng đã bị ao bao phủ, số chia được coi là không hợp lệ và quá trình quét có thể dừng lại. 

Vòng lặp cuối cùng sử dụng`math.gcd`trực tiếp. Số nguyên Python không gặp vấn đề tràn ở đây vì tất cả các giá trị tối đa là (10^5). 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đấu trường là`RRR`. Không có ao nên mọi độ dài bước nhảy có thể đều có giá trị ngay lập tức. 

| (K) | (\gcd(K,3)) | Có hiệu lực? | Lý do | 
| --- | --- | --- | --- | 
| 1 | 1 | Có | Mỗi vị trí được ghé thăm là một tảng đá | 
| 2 | 1 | Có | Mỗi vị trí được ghé thăm là một tảng đá | 

Câu trả lời là`2`. Dấu vết này thể hiện lối tắt toàn đá. 

Đối với Mẫu 2, đấu trường là`RRPR`. 

| (K) | (\gcd(K,4)) | Dư lượng được truy cập từ một khởi đầu thích hợp | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 1 | Tất cả các vị trí | Không, vị trí 2 là`P`| 
| 2 | 2 | Một lớp dư lượng modulo 2 | Có, vị trí 0 và 2 là`R`| 
| 3 | 1 | Tất cả các vị trí | Không, vị trí 2 là`P`| 

Với (g=2), hai lớp dư lượng là ({0,2}) và ({1,3}). Ao nằm ở vị trí (2) nên lớp thứ nhất không sử dụng được, còn lớp thứ hai toàn là đá. Do đó (K=2) hoạt động và câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\tau(N)+N\log N)) | Mỗi ước số có thể kiểm tra các vị trí ao và mỗi (K) cần một phép tính gcd | 
| Không gian | (O(N)) | Vị trí ao, cờ chia và điểm đánh dấu dư lượng sử dụng bộ nhớ tuyến tính | 

Ở đây (\tau(N)) là số ước của (N). Đối với (N\le10^5), giá trị này nhỏ, tối đa là 128 trong phạm vi này. Khối lượng công việc thu được dễ dàng thực tế đối với giới hạn chính thức (N\le10^5), 1 giây, 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from math import gcd, isqrt

    def solve_string(s: str) -> int:
        n = len(s)
        ponds = [i for i, ch in enumerate(s) if ch == 'P']

        if not ponds:
            return n - 1

        divisors = []
        for d in range(1, isqrt(n) + 1):
            if n % d == 0:
                divisors.append(d)
                if d * d != n:
                    divisors.append(n // d)

        can_jump = [False] * (n + 1)
        pond_count = len(ponds)

        for g in divisors:
            if g == n:
                continue

            if pond_count < g:
                can_jump[g] = True
                continue

            seen = bytearray(g)
            covered = 0

            for p in ponds:
                r = p % g
                if not seen[r]:
                    seen[r] = 1
                    covered += 1
                    if covered == g:
                        break

            can_jump[g] = covered < g

        answer = 0
        for k in range(1, n):
            if can_jump[gcd(k, n)]:
                answer += 1

        return answer

    s = inp.strip()
    return str(solve_string(s)) + "\n"

# Provided samples
assert run("RRR\n") == "2\n", "sample 1"
assert run("RRPR\n") == "1\n", "sample 2"
assert run("PRP\n") == "0\n", "sample 3"

# Minimum-size case, with no rock at all.
assert run("PPP\n") == "0\n", "minimum size, all ponds"

# Starting position need not be 0.
assert run("PRRR\n") == "1\n", "valid start is position 1"

# Boundary case where gcd(K, N) = 2 is the only valid class.
assert run("RRPR\n") == "1\n", "gcd class boundary"

# Maximum-size case.
assert run("R" * 100000 + "\n") == "99999\n", "maximum size, all rocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`PPP`|`0`| Kích thước tối thiểu và không có bất kỳ đá khởi đầu hợp pháp nào | 
|`PRRR`|`1`| Vị trí bắt đầu có thể khác với vị trí 0 | 
|`RRPR`|`1`| Xử lý đúng các lớp dư lượng và gcd | 
|`R`lặp đi lặp lại 100000 lần |`99999`| Kích thước đầu vào tối đa và phím tắt toàn đá | 

## Vỏ cạnh 

cho`PRRR`, đầu ra đúng là`1`. Độ dài bước nhảy hữu ích duy nhất là (K=2). Gcd của nó với (N=4) là (2), do đó nó thăm một trong hai lớp dư lượng modulo 2. Bắt đầu từ vị trí (1) cho ra chu trình (1\rightarrow3\rightarrow1), chỉ chứa đá. Bắt đầu từ vị trí (0) sẽ thất bại vì vị trí (0) là một cái ao. Thuật toán không chọn điểm bắt đầu cố định nên phát hiện chính xác lớp dư lượng tốt. 

Vì`PPP`, đầu ra đúng là`0`. Mọi lớp cặn có thể đều chứa một cái ao vì mọi vị trí đều là một cái ao. Đối với mỗi ước số thích hợp (g), tất cả (g) các loại dư lượng được đánh dấu theo vị trí ao, do đó`can_jump[g]`vẫn sai. Không có chiều dài bước nhảy được tính. 

Vì`RRR`, đầu ra đúng là`2`. Thuật toán đạt đến lối tắt toàn đá trước khi thực hiện bất kỳ xử lý số chia nào và trả về (N-1=2). Điều này cũng bao gồm trường hợp một cú nhảy có thể ghé thăm mọi vị trí, bởi vì không có ao nào để coi việc truy cập như vậy là bất hợp pháp. 

Vì`RRPR`, đầu ra đúng là`1`. Số chia (g=1) không hợp lệ vì lớp dư lượng duy nhất của nó chứa ao ở vị trí (2). Số chia (g=2) hợp lệ vì phần dư (1) chứa vị trí (1) và (3), cả hai loại đá. Ước số (g=4) bị bỏ qua vì không (K<N) có thể có gcd (4) với (N=4). Trong (K=1,2,3) chỉ có (K=2) có gcd (2) nên đáp án cuối cùng là chính xác`1`.
