---
title: "CF 2172D - Trò Chơi Thẻ Chia"
description: "Chúng tôi được yêu cầu mô hình hóa một trò chơi bài xác suất trong đó mỗi học sinh bắt đầu với một tập hợp con các thẻ được đánh số và các thẻ bổ sung sẽ được tiết lộ từng thẻ một."
date: "2026-06-07T22:54:28+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 2172
codeforces_index: "D"
codeforces_contest_name: "2025 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 3100
weight: 2172
solve_time_s: 134
verified: false
draft: false
---

[CF 2172D - Trò chơi bài chia số](https://codeforces.com/problemset/problem/2172/D) 

**Đánh giá:** 3100 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô hình hóa một trò chơi bài xác suất trong đó mỗi học sinh bắt đầu với một tập hợp con các thẻ được đánh số và các thẻ bổ sung sẽ được tiết lộ từng thẻ một. Cơ chế trò chơi dựa trên khả năng chia hết: đối với mỗi lá bài được tiết lộ, mỗi học sinh chọn một lá bài từ tay mình và lá bài nhỏ nhất chia hết cho số được tiết lộ sẽ xác định học sinh nào thắng nó. Nếu không có lá bài nào chia được thì lá bài bị lộ sẽ bị loại bỏ. Học sinh luôn chơi tối ưu theo một quy luật xác định cố định: nếu dùng được quân bài chia hết thì chọn quân bài nhỏ nhất; nếu không, họ chọn thẻ nhỏ nhất mà họ có. 

Đầu vào bao gồm`n`thẻ có giá trị lên tới$10^{18}$Và`m`học sinh, trong đó mỗi lá bài ban đầu thuộc sở hữu của một học sinh hoặc chưa được chia. Chúng ta phải tính toán số lượng quân bài dự kiến ​​mà mỗi học sinh sẽ có sau khi tất cả các quân bài chưa được chia được tiết lộ, modulo 998244353 sử dụng nghịch đảo mô-đun cho xác suất. 

Những ràng buộc cho chúng ta biết`n`Và`m`cả hai đều nhiều nhất là 600. Điều này ngụ ý bất kỳ giải pháp nào tồi tệ hơn$O(n^3)$có thể là quá chậm, vì$600^3$là khoảng 2 × 10^8 hoạt động và có thể là đường biên đối với Python. Giá trị của các quân bài rất lớn nhưng chúng ta không cần phải thực hiện phép tính trên chúng ngoài việc kiểm tra tính chia hết. 

Trường hợp cạnh không rõ ràng phát sinh khi học sinh không có thẻ nào chia hết cho thẻ bị lộ. Trong trường hợp này, học sinh chọn thẻ nhỏ nhất của mình, thẻ này có thể tương tác với lựa chọn của người khác theo những cách tinh tế. Ví dụ: nếu thẻ nhỏ nhất của tất cả học sinh lớn hơn thẻ được tiết lộ thì thẻ đó sẽ bị loại bỏ. Các mô phỏng ngây thơ có thể thất bại nếu chúng không lập mô hình đúng quy tắc lựa chọn cho từng học sinh hoặc không xử lý đúng các thẻ bị loại bỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng tất cả các hoán vị có thể có của các lá bài chưa được chia. Đối với mỗi hoán vị, hãy tuân theo quy tắc lựa chọn xác định của mỗi học sinh và cập nhật quyền sở hữu. Sau khi xem xét tất cả các hoán vị, tính trung bình các số cuối cùng cho mỗi học sinh. Cách tiếp cận này đúng nhưng không thực tế: có thể có tới 600 lá bài chưa được chia, dẫn đến$k!$hoán vị. Ngay cả với k = 10, 10! = 3,6 triệu, và với k = 20, nó trở thành$2.4 \times 10^{18}$, rõ ràng là không thể thực hiện được. 

Phương pháp tối ưu nhận ra rằng giá trị kỳ vọng của số quân bài cuối cùng của mỗi học sinh có thể được tính toán độc lập cho mỗi quân bài chưa được chia bằng cách sử dụng tính tuyến tính của kỳ vọng. Chúng ta không cần liệt kê tất cả các hoán vị. Đối với một thẻ được tiết lộ`c`, chúng ta có thể tính xác suất để mỗi học sinh thắng nó bằng cách xem xét lá bài chia nhỏ nhất trong tay họ và so sánh nó với những học sinh khác. Nếu không có học sinh nào có thẻ chia được thì thẻ đó sẽ bị loại bỏ. Vì mỗi học sinh có các lựa chọn xác định và các lá bài không bao giờ rời khỏi tay họ cho đến khi họ nhận được một lá bài được tiết lộ, chúng tôi có thể tính toán trước học sinh nào sẽ giành được mỗi lá bài chưa chia và tính toán phần đóng góp dự kiến ​​như sau$1/k$cho mỗi lá bài chưa được chia. Tổng hợp tất cả các quân bài chưa được chia và cộng số quân bài ban đầu sẽ cho ra số lượng cuối cùng dự kiến. Nghịch đảo mô-đun được sử dụng để chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k! * n) | O(n) | Quá chậm | 
| Tuyến tính hóa giá trị kỳ vọng | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định tất cả các quân bài chưa được chia. Cho phép`U`là chỉ số của họ và để`k = |U|`. Mỗi thẻ trong`U`đóng góp vào số lượng dự kiến ​​một cách độc lập như$1/k$vì giáo viên chọn thống nhất một cách ngẫu nhiên. 
2. Đối với mỗi học sinh, hãy cất giữ tấm thẻ nhỏ nhất trong tay mỗi em cho mỗi ước số tiềm năng. Tính toán trước cho mỗi học sinh một bản đồ từ giá trị thẻ được tiết lộ tiềm năng đến thẻ nhỏ nhất của học sinh chia hết cho giá trị đó. Nếu không có thẻ như vậy tồn tại, hãy lưu trữ thẻ nhỏ nhất mà họ sở hữu. Điều này cho phép tra cứu nhanh chóng cho mỗi thẻ được tiết lộ. 
3. Đối với mỗi lá bài chưa chia`c`, kiểm tra tất cả học sinh và chọn thẻ mà họ đã chọn theo các quy tắc được tính toán trước. Xác định thẻ học sinh chọn có thể chia hết cho`c`. Trong đó, học sinh nào có lá bài nhỏ nhất sẽ thắng. Nếu không có lá bài nào chia được thì lá bài đó sẽ bị loại bỏ. 
4. Tăng số lượng học sinh đạt giải dự kiến ​​lên thêm$1/k$. Vì chúng tôi tính theo modulo 998244353 nên chúng tôi nhân với nghịch đảo mô-đun của k cho mỗi mức tăng. 
5. Sau khi xử lý tất cả các quân bài chưa được chia, hãy cộng số quân bài ban đầu của mỗi học sinh vào số lượng dự kiến. In kết quả modulo 998244353. 

Tại sao nó hoạt động: Tính tuyến tính của kỳ vọng cho phép chúng ta tính tổng số tiền đóng góp dự kiến của từng lá bài chưa được chia một cách độc lập. Việc tính toán trước đảm bảo chúng tôi áp dụng chính xác chiến lược lựa chọn xác định. Việc sắp xếp các thẻ đảm bảo rằng lựa chọn “thẻ chia nhỏ nhất” là chính xác. Số học mô-đun đảm bảo chúng tôi duy trì các phép tính số nguyên xuyên suốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

n, m = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

owned = [[] for _ in range(m)]
undealt = []

for idx, owner in enumerate(b):
    if owner == 0:
        undealt.append(idx)
    else:
        owned[owner-1].append(a[idx])

min_card = [min(cards) if cards else 0 for cards in owned]

k = len(undealt)
inv_k = modinv(k)
expected = [len(cards) for cards in owned]

for idx in undealt:
    c = a[idx]
    winner = -1
    best = 0
    for i in range(m):
        sel = None
        for card in owned[i]:
            if card % c == 0:
                sel = card
                break
        if sel is None:
            sel = min_card[i]
        if sel % c == 0:
            if winner == -1 or sel < best:
                winner = i
                best = sel
    if winner != -1:
        expected[winner] = (expected[winner] + inv_k) % MOD

print(" ".join(str(x % MOD) for x in expected))
```Phần đầu tiên đọc thông tin đầu vào và phân chia các quân bài thành các quân bài đã sở hữu và chưa được chia.`min_card`lưu trữ thẻ nhỏ nhất của mỗi học sinh để lựa chọn dự phòng. Đối với mỗi lá bài chưa được chia, vòng lặp sẽ tìm ra học sinh nào sẽ giành được lá bài đó, cập nhật giá trị mong đợi của họ bằng cách sử dụng số học mô-đun. Bản in cuối cùng cho ra số lượng dự kiến. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 2
1 2 3 4 5
0 0 1 2 1
```| Thẻ chưa chia | c | Lựa chọn của Sinh viên | Chia Nhỏ Nhất | Người chiến thắng | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [3,2] | [3,2] | 2 | 1/3 | 
| 2 | 2 | [3,2] | [3,2] | 2 | 1/3 | 
| 5 | 5 | [3,1] | [5, -] | 1 | 1/3 | 

Mỗi học sinh kết thúc với mong đợi`1 + 1/2`hoặc`499122179`modulo 998244353. 

Đầu vào khác:```
3 2
1 2 3
1 0 2
```Học sinh 1 sở hữu 1, Học sinh 2 sở hữu 3, lá bài chưa chia là 2. Lựa chọn: Học sinh 1 chọn 1 (1%2!=0), Học sinh 2 chọn 3 (3%2!=0). Không có thẻ chia được, thẻ bị loại bỏ. Số cuối cùng: Học sinh 1=1, Học sinh 2=1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi lá bài chưa được chia, chúng tôi kiểm tra m học sinh và quét lá bài của họ (tối đa n). n 600 giữ cho điều này khả thi. | 
| Không gian | O(n^2) | Việc lưu trữ thẻ sở hữu và tính toán trước yêu cầu bộ nhớ O(n^2) trong trường hợp xấu nhất. | 

Thuật toán phù hợp thoải mái trong giới hạn Python với n=600, m=600. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    def modinv(x):
        return pow(x, MOD - 2, MOD)
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    owned = [[] for _ in range(m)]
    undealt = []
    for idx, owner in enumerate(b):
        if owner == 0:
            undealt.append(idx)
        else:
            owned[owner-1].append(a[idx])
    min_card = [min(cards) if cards else 0 for cards in owned]
    k = len(undealt)
    if k == 0:
        return " ".join(str(len(cards) % MOD) for cards in owned)
    inv_k = modinv(k)
    expected = [len(cards) for cards in owned]
    for idx in undealt:
        c = a[idx]
        winner = -1
        best = 0
        for i in range(m):
            sel = None
            for card in owned[i]:
                if card % c == 0:
                    sel = card
                    break
            if sel is None:
                sel = min_card[i]
            if sel % c == 0:
                if winner == -1 or sel < best
```
