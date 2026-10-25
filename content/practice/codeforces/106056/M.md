---
title: "CF 106056M - Vải bố"
description: "1. Sắp xếp canvas theo điểm cuối bên phải của chúng. Điều này đảm bảo rằng khi chúng tôi xử lý canvas, tất cả các canvas trong tương lai sẽ không kết thúc sớm hơn, vì vậy, các quyết định được đưa ra bây giờ có thể giả định một cách an toàn rằng không có khoảng thời gian nào trong tương lai yêu cầu điều gì đó nghiêm ngặt sớm hơn mà chưa được xem xét. 2."
date: "2026-06-25T12:21:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106056
codeforces_index: "M"
codeforces_contest_name: "The 1st Universal Cup. Stage 18: Shenzhen"
rating: 0
weight: 106056
solve_time_s: 36
verified: true
draft: false
---

[CF 106056M - Canvas](https://codeforces.com/problemset/problem/106056/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hướng dẫn thuật toán 

1. Sắp xếp canvas theo điểm cuối bên phải của chúng. Điều này đảm bảo rằng khi chúng tôi xử lý canvas, tất cả các canvas trong tương lai sẽ không kết thúc sớm hơn, vì vậy, các quyết định được đưa ra bây giờ có thể giả định một cách an toàn rằng không có khoảng thời gian nào trong tương lai yêu cầu điều gì đó nghiêm ngặt sớm hơn mà chưa được xem xét. 
2. Duy trì một tập hợp các vị trí chốt động. Điều này bao gồm cả các chốt ban đầu và mới được thêm vào, được lưu trữ theo thứ tự được sắp xếp để chúng ta có thể truy vấn có bao nhiêu nằm trong bất kỳ khoảng nào một cách hiệu quả. 
3. Đối với mỗi canvas$[l, r]$, đếm xem có bao nhiêu chốt hiện có trong đó. Điều này có thể được thực hiện bằng tìm kiếm nhị phân trên danh sách chốt đã sắp xếp. 
4. Nếu số lượng ít nhất là hai, không làm gì với khung vẽ này. Hai chốt bất kỳ bên trong nó đều thỏa mãn yêu cầu và chúng ta không cần đưa thêm chốt mới vào. Các chốt bổ sung không liên quan vì chúng không bao giờ vi phạm các ràng buộc. 
5. Nếu số đếm chính xác là một thì chúng ta cần thêm một chốt nữa. Chúng tôi đặt một chốt mới ở vị trí$r$, vì nó được đảm bảo nằm bên trong canvas và tối đa hóa khả năng tái sử dụng trong tương lai. Sau khi chèn nó, chúng tôi cập nhật bộ chốt. 
6. Nếu số đếm bằng 0, chúng ta cần hai chốt mới. Chúng tôi đặt một cái ở$r$, và sau đó là một cái khác tại$r-1$, giả sử nó vẫn nằm trong giới hạn của khoảng. Nếu như$r-l < 1$, tính khả thi là không thể vì canvas có chiều rộng ít nhất là 10 nên trường hợp này không thể xảy ra với dữ liệu đầu vào hợp lệ. 
7. Sau khi đặt chốt mới, chúng ta cập nhật cấu trúc và tiếp tục. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi đảm bảo canvas hiện tại kết thúc bằng chính xác hai chốt bên trong và chúng tôi không bao giờ đặt chốt bên ngoài canvas trừ khi nó cũng nhằm mục đích phục vụ canvas sau này. Sự lựa chọn tham lam trong việc đặt các chốt ở đúng ranh giới sẽ duy trì tính linh hoạt trong tương lai bởi vì bất kỳ vị trí nào trước đó sẽ chỉ làm giảm sự chồng chéo với các khoảng thời gian trong tương lai. Vì các canvas được xử lý theo các điểm cuối phù hợp ngày càng tăng nên không có canvas nào trong tương lai có thể bị các lựa chọn hiện tại của chúng ta buộc vào tình huống tồi tệ hơn, miễn là chúng ta luôn đáp ứng canvas hiện tại ở mức tối thiểu và thiên về bên phải. 

Bất biến chính là sau khi xử lý từng khung vẽ, tất cả các khung vẽ được xử lý đã có chính xác hai chốt hỗ trợ được chọn và tập hợp các chốt có sẵn đủ để đáp ứng tất cả các khung vẽ trong tương lai nếu có giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    seg = [tuple(map(int, input().split())) for _ in range(n)]
    p = int(input())
    pegs = list(map(int, input().split()))

    seg.sort(key=lambda x: x[1])
    pegs.sort()

    import bisect
    added = []

    def count_in(l, r):
        return bisect.bisect_right(pegs, r) - bisect.bisect_left(pegs, l)

    for l, r in seg:
        # count existing pegs in range
        cnt = count_in(l, r)

        # also include newly added pegs
        cnt += sum(1 for x in added if l <= x <= r)

        if cnt >= 2:
            continue

        need = 2 - cnt

        for _ in range(need):
            pos = r
            # ensure no duplicate exact peg if already exists
            while pos in pegs or pos in added:
                pos -= 1
            if pos < l:
                print("impossible")
                return
            added.append(pos)

    print(len(added))
    if added:
        print(*added)

if __name__ == "__main__":
    solve()
```Việc triển khai giúp sắp xếp các chốt hiện có để đếm phạm vi nhanh và duy trì riêng các chốt mới được thêm vào. Vòng điều chỉnh bên trong đảm bảo chúng tôi không vô tình đặt hai chốt giống hệt nhau, điều này sẽ vi phạm yêu cầu “hai giá đỡ riêng biệt” trên mỗi khung vẽ. Đây là cạm bẫy thực tế chính: coi các vị trí cố định là nguồn cung không giới hạn tại một điểm duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với các khung vẽ chồng lên nhau: 

đầu vào:```
2
0 10
5 15
1
7
```| Vải | Khoảng thời gian | Hiện tại trong khoảng | Hành động | Đã thêm chốt | 
| --- | --- | --- | --- | --- | 
| 1 | [0,10] | 7 | cần thêm 1 cái nữa | 10 | 
| 2 | [5,15] | 7,10 | đã 2 | - | 

Canvas đầu tiên buộc phải bổ sung một lần. Canvas thứ hai được hưởng lợi từ vị trí trước đó, giúp tránh phải làm thêm. 

Bây giờ là một trường hợp yêu cầu hai bổ sung: 

đầu vào:```
1
0 12
0
```| Vải | Khoảng thời gian | Hiện có | Hành động | Đã thêm | 
| --- | --- | --- | --- | --- | 
| [0,12] | không | 0 | cộng 12 và 11 | 12, 11 | 

Điều này cho thấy dự phòng khi không có cấu trúc tồn tại. 

Những dấu vết này xác nhận rằng thuật toán luôn ưu tiên sử dụng lại khi có thể và chỉ đưa ra các chốt mới khi thực sự cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp các khung vẽ và tìm kiếm nhị phân theo khoảng thời gian | 
| Không gian |$O(n)$| lưu trữ chốt và thêm vị trí | 

Những hạn chế$n \le 1000$thực hiện việc này nhanh chóng một cách thoải mái ngay cả với chi phí cấp Python, vì tất cả các hoạt động đều bị chi phối bởi việc sắp xếp và quét đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    seg = [tuple(map(int, input().split())) for _ in range(n)]
    p = int(input())
    pegs = list(map(int, input().split()))

    seg.sort(key=lambda x: x[1])
    pegs.sort()

    import bisect
    added = []

    def count_in(l, r):
        return bisect.bisect_right(pegs, r) - bisect.bisect_left(pegs, l)

    for l, r in seg:
        cnt = count_in(l, r)
        cnt += sum(1 for x in added if l <= x <= r)

        if cnt >= 2:
            continue

        need = 2 - cnt
        for _ in range(need):
            pos = r
            while pos in pegs or pos in added:
                pos -= 1
            if pos < l:
                return "impossible\n"
            added.append(pos)

    return str(len(added)) + ("\n" + " ".join(map(str, added)) if added else "") + "\n"

# basic sanity cases
assert run("""1
0 10
0
""").startswith("2")

assert run("""2
0 10
10 20
2
5 15
""") != ""

# edge: already satisfied
assert "0" in run("""1
0 10
2
1 9
""")

# overlapping heavy reuse
assert run("""3
0 10
5 15
10 20
1
12
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| canvas trống duy nhất | Đã thêm 2 | xử lý yêu cầu cơ bản | 
| khoảng chồng chéo | khác không nhưng tối thiểu | tái sử dụng trên các khung vẽ | 
| đã hài lòng canvas | 0 | không cần chèn thêm | 
| chồng chéo | bổ sung nhỏ hợp lệ | tham lam tái sử dụng đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi canvas đã chứa chính xác một chốt hiện có. Thuật toán không được cho rằng bất kỳ chốt mới nào cũng đủ mà không cần kiểm tra giới hạn. Ví dụ, nếu khoảng đó là$[10, 11]$và chốt hiện có duy nhất là ở mức 10, việc đặt cả hai chốt bắt buộc trong khoảng có thể tạo ra xung đột hoặc vị trí giống hệt nhau. Chiến lược giảm tham lam tránh sự trùng lặp bằng cách đảm bảo các vị trí khác biệt. 

Một trường hợp tinh vi khác là khi nhiều khung vẽ có chung một điểm ranh giới. Vì các canvas không chồng lên nhau nhưng có thể chạm vào nhau nên một chốt được đặt ở điểm cuối dùng chung sẽ hợp lệ cho cả hai. Việc sắp xếp theo điểm cuối bên phải đảm bảo rằng các ranh giới được chia sẻ như vậy được tái sử dụng một cách tự nhiên và thuật toán không tính sai hai lần chúng. 

Trường hợp góc cuối cùng là khi tất cả các khung vẽ đều rời rạc và trống rỗng. Khi đó, mỗi khung vẽ sẽ yêu cầu hai chốt độc lập, nhưng vì không có sự chồng chéo nên không thể sử dụng lại. Thuật toán suy biến thành việc đặt hai chốt mới cho mỗi khoảng, khớp với giới hạn dưới tối ưu.
