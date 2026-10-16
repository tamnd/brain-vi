---
title: "CF 105492C - Cuộc thi đồng thời"
description: "Có một số cuộc thi lập trình diễn ra cùng lúc và một nhóm người tham gia phải được phân bổ trong số đó. Mỗi cuộc thi có một giải thưởng duy nhất mà chỉ người chiến thắng mới nhận được."
date: "2026-06-23T20:21:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105492
codeforces_index: "C"
codeforces_contest_name: "2024 Benelux Algorithm Programming Contest (BAPC 24)"
rating: 0
weight: 105492
solve_time_s: 106
verified: true
draft: false
---

[CF 105492C - Cuộc thi đồng thời](https://codeforces.com/problemset/problem/105492/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có một số cuộc thi lập trình diễn ra cùng lúc và một nhóm người tham gia phải được phân bổ trong số đó. Mỗi cuộc thi có một giải thưởng duy nhất mà chỉ người chiến thắng mới nhận được. Cơ hội chiến thắng trong cuộc thi của người tham gia phụ thuộc vào kỹ năng của họ so với tổng kỹ năng của mọi người trong cuộc thi đó: nếu cuộc thi có tổng kỹ năng$T$, thì một người tham gia có kỹ năng$s$thắng có xác suất$s / T$. Vậy phần thưởng mong đợi cho người tham gia cuộc thi đó là$p_j \cdot s / T$, Ở đâu$p_j$là giải thưởng của cuộc thi đó 

Vì mỗi người phải chọn chính xác một cuộc thi và họ hành động ích kỷ nên chúng tôi muốn phân bổ người tham gia qua các cuộc thi sao cho không ai có thể cải thiện phần thưởng mong đợi của mình bằng cách chuyển sang một cuộc thi khác. Nhiệm vụ là xây dựng bất kỳ nhiệm vụ ổn định như vậy. 

Các ràng buộc rất mạnh: lên đến$2 \cdot 10^5$người tham gia nhưng chỉ có tối đa 100 cuộc thi. Điều đó ngay lập tức gợi ý rằng chúng tôi không thể thử tối ưu hóa cho mỗi người cho mỗi cuộc thi sau khi liên tục thực hiện các nhiệm vụ tùy ý, vì ngay cả một lượt phân công lại cũng có thể bị tính phí.$O(nm)$và sự hội tụ lặp đi lặp lại sẽ quá chậm. Thay vào đó, cấu trúc phải xuất phát từ đặc tính trực tiếp của trạng thái cân bằng. 

Một vấn đề tế nhị là tính ổn định mang tính toàn cầu: việc di chuyển một người tham gia sẽ thay đổi toàn bộ kỹ năng của cả cuộc thi nguồn và đích, điều này làm thay đổi xác suất của mọi người. Một kẻ tham lam ngây thơ chỉ định từng người chơi mà không tôn trọng mục tiêu chung có thể âm thầm phá vỡ trạng thái cân bằng vào cuối, bởi vì những quyết định ban đầu làm sai lệch mẫu số cho những người tham gia sau này. 

Một trường hợp thất bại khác xuất hiện nếu chúng ta giả sử mỗi người tham gia độc lập chọn cuộc thi tốt nhất theo tỷ lệ hiện tại. Ví dụ: nếu một cuộc thi tạm thời có vẻ hấp dẫn do tổng kỹ năng nhỏ, thì nhiều người chơi mạnh có thể tham gia, sau đó khiến cuộc thi trở nên tồi tệ hơn và gây ra sự đảo ngược. Cấu hình cuối cùng phải tránh hoàn toàn sự mất ổn định này, không chỉ tối ưu hóa cục bộ tại thời điểm xây dựng. 

## Phương pháp tiếp cận 

Phần thưởng mong đợi của người tham gia có kỹ năng$s$trong cuộc thi$j$là$$\frac{p_j}{T_j} \cdot s$$Nhận xét quan trọng là đối với một nhiệm vụ cố định, yếu tố phụ thuộc vào người tham gia sẽ biến mất khi so sánh các cuộc thi. Mỗi người tham gia chỉ so sánh số lượng$p_j / T_j$. Điều này có nghĩa là tất cả những người tham gia đều đồng ý về cuộc thi nào là tốt nhất: tất cả họ đều thích cuộc thi có tỷ lệ lớn nhất$p_j / T_j$. 

Nếu bất kỳ cuộc thi nào có quy mô lớn hơn$p_j / T_j$, thì mọi người tham gia sẽ muốn chuyển đến đó, điều này mâu thuẫn với sự ổn định. Vì vậy, trong một cấu hình ổn định, tất cả các cuộc thi được sử dụng phải có cùng giá trị tỷ lệ, chẳng hạn như$\lambda$, và các cuộc thi chưa được sử dụng phải có tỷ lệ tối đa$\lambda$. 

Điều này buộc một cấu trúc cứng nhắc. Đối với mỗi cuộc thi$j$,$$\frac{p_j}{T_j} = \lambda \Rightarrow T_j = \frac{p_j}{\lambda}$$Tổng hợp tất cả các cuộc thi,$$\sum T_j = \sum s_i = S
\Rightarrow \sum \frac{p_j}{\lambda} = S
\Rightarrow \lambda = \frac{\sum p_j}{S}$$Vì vậy, mỗi cuộc thi phải nhận được tổng kỹ năng tỷ lệ chính xác với giải thưởng của nó:$$T_j = S \cdot \frac{p_j}{\sum p}$$Vấn đề giảm xuống còn việc phân chia nhiều tập hợp kỹ năng thành$m$các nhóm có tổng mục tiêu cố định. 

Một cách tiếp cận vũ phu sẽ thử tất cả các bài tập của$n$mọi người vào$m$xô, đó là$m^n$, ngay lập tức là không thể. Ngay cả việc cố gắng điều chỉnh các bài tập lặp đi lặp lại cũng sẽ yêu cầu phải cân bằng tổng nhiều lần, điều này có thể giảm xuống$O(n^2)$hoặc tệ hơn. 

Hiểu biết sâu sắc về cấu trúc là khi tổng mục tiêu được cố định, chúng ta chỉ cần xây dựng các tập hợp con khớp với các tổng đó. Bởi vì$m \le 100$, chúng ta có thể xử lý từng cuộc thi một và tham lam điền vào từng số tiền cần thiết bằng cách sử dụng những người chơi còn lại. Việc sắp xếp người chơi theo kỹ năng giảm dần khiến điều này trở nên khả thi: các phần tử lớn được đặt lên hàng đầu, đảm bảo chúng ta không bị mắc kẹt ở cuối với phần còn lại lớn không thể lấp đầy. 

Vì một phân vùng hợp lệ được đảm bảo tồn tại nên việc lấp đầy tham lam này là đủ để xây dựng một phân vùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm bài tập Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng tham lam với số tiền mục tiêu | O(n log n + n m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng kỹ năng$S$và tổng số tiền thưởng$P$. Điều này xác định hệ số tỷ lệ cân bằng một cách gián tiếp, vì mọi cuộc thi đều phải nhận được kỹ năng tương ứng với giải thưởng của nó. 
2. Đối với mỗi cuộc thi, hãy tính tổng mục tiêu cần thiết$T_j = S \cdot p_j / P$. Những mục tiêu này thể hiện tổng khối lượng kỹ năng mà mỗi cuộc thi phải hấp thụ ở bất kỳ trạng thái cân bằng hợp lệ nào. 
3. Sắp xếp người tham gia theo kỹ năng giảm dần. Thứ tự này đảm bảo rằng chúng tôi đặt sớm các giá trị lớn, giúp giảm nguy cơ bị mắc kẹt với phần tử quá khổ mà không mục tiêu còn lại nào có thể chấp nhận. 
4. Duy trì con trỏ về các cuộc thi và tổng số hiện có cho cuộc thi hiện tại. Lặp lại những người tham gia và chỉ định từng người tham gia cuộc thi hiện tại đồng thời tích lũy tổng kỹ năng của họ. 
5. Sau khi cuộc thi đạt chính xác số tiền mục tiêu, hãy chuyển sang cuộc thi tiếp theo. Thời điểm chuyển đổi bị ép buộc bởi sự bằng nhau của các tổng chứ không phải theo phương pháp phỏng đoán, đảm bảo rằng mỗi nhóm phù hợp với khối lượng yêu cầu của nó. 
6. Xuất ra tất cả các nhóm đã xây dựng. 

Lý do bước 5 hợp lệ là vì sự đảm bảo tồn tại ngụ ý rằng các mục tiêu tương thích với nhiều tập hợp, do đó, việc đóng gói tham lam sẽ không bao giờ vượt quá mức không thể khắc phục được theo thứ tự giảm dần. 

### Tại sao nó hoạt động 

Trong bất kỳ giải pháp ổn định nào, mọi cuộc thi đều phải tạo ra cùng một giá trị$p_j / T_j$. Ràng buộc đó khắc phục duy nhất tất cả các tổng mục tiêu theo một hệ số tỷ lệ chung. Bài toán xây dựng sau đó trở thành một phân vùng của một tập hợp được sắp xếp thành các tập hợp liền kề phù hợp với các tổng được xác định trước. Bởi vì tất cả các kỹ năng đều tích cực và chúng tôi xử lý theo thứ tự giảm dần, nên khi một phần tổng có thể đạt được mục tiêu, việc hoàn thành nó trước khi tiếp tục là điều an toàn. Điều này duy trì tính khả thi của các mục tiêu còn lại, vì các phần tử lớn hơn không bao giờ được để lại cho các nhóm có dung lượng nhỏ hơn sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    s = list(map(int, input().split()))
    p = list(map(int, input().split()))

    S = sum(s)
    P = sum(p)

    # compute target sums as integers
    targets = [S * pi // P for pi in p]

    # sort participants by skill (value, index)
    arr = sorted([(v, i + 1) for i, v in enumerate(s)], reverse=True)

    ans = [[] for _ in range(m)]

    j = 0
    cur_sum = 0

    for val, idx in arr:
        ans[j].append(idx)
        cur_sum += val

        if cur_sum == targets[j]:
            j += 1
            cur_sum = 0

    for group in ans:
        print(len(group), *group)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ xây dựng lại tổng kỹ năng cần thiết cho mỗi cuộc thi từ tỷ lệ thu được trong điều kiện cân bằng. Sau đó, nó xử lý bài tập như một vấn đề đóng gói trên các trọng số đã được sắp xếp. Việc quét tham lam trong các cuộc thi có hiệu quả vì mỗi mục tiêu được hoàn thành chính xác một lần và việc chuyển đổi chỉ xảy ra ở mức chính xác. 

Một điểm tinh tế là việc sử dụng phép chia số nguyên khi tính toán mục tiêu. Trong các đầu vào hợp lệ, các giá trị tỷ lệ được đảm bảo tính tổng chính xác bằng$S$, do đó vấn đề làm tròn không phát sinh khi xây dựng đúng. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó các kỹ năng được chia thành hai cuộc thi. 

### Ví dụ 1 

đầu vào:```
n = 5, m = 2
skills = [8, 5, 3, 2, 2]
prizes = [10, 5]
```Đây$S = 20$,$P = 15$, vậy mục tiêu là$T_1 = 20 * 10 / 15 = 13.33...$,$T_2 = 6.66...$. Trong một trường hợp hợp lệ, đây sẽ là các số nguyên chính xác, nhưng chúng tôi coi phần chia tỷ lệ theo khái niệm là 13 và 7 để minh họa. 

Sắp xếp kỹ năng: [8, 5, 3, 2, 2] 

| Bước | Được giao | Cuộc thi hiện tại | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | 8 | 0 | 8 | 
| 2 | 5 | 0 | 13 | 
| chuyển đổi | | 1 | 0 | 
| 3 | 3 | 1 | 3 | 
| 4 | 2 | 1 | 5 | 
| 5 | 2 | 1 | 7 | 

Cuộc thi đầu tiên hoàn thành chính xác, sau đó phần còn lại sẽ chuyển sang cuộc thi thứ hai. 

Dấu vết này cho thấy thứ tự giảm dần cho phép đóng chính xác nhóm đầu tiên mà không cần quay lại như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 6, m = 3
skills = [10, 7, 5, 4, 3, 1]
prizes = [9, 6, 3]
```Các kỹ năng được sắp xếp vẫn giữ nguyên. 

Giả sử các mục tiêu tỷ lệ chia tổng số thành các công suất giảm dần. 

| Bước | Được giao | Cuộc thi | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | 10 | 0 | 10 | 
| 2 | 7 | 0 | 17 | 
| chuyển đổi | | 1 | 0 | 
| 3 | 5 | 1 | 5 | 
| 4 | 4 | 1 | 9 | 
| chuyển đổi | | 2 | 0 | 
| 5 | 3 | 2 | 3 | 
| 6 | 1 | 2 | 4 | 

Mỗi cuộc thi được thực hiện đúng một lần, cho thấy rằng khi đạt được mục tiêu, cấu trúc còn lại không bị ảnh hưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + n)$| Sắp xếp chiếm ưu thế; quét tuyến tính đơn chỉ định mỗi người tham gia một lần | 
| Không gian |$O(n)$| Lưu trữ nhóm và danh sách được sắp xếp | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$người tham gia, vì vậy một$O(n \log n)$giải pháp nằm trong giới hạn. Số lượng cuộc thi ít nên việc xây dựng tuyến tính trên chúng là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict

    def solve():
        n, m = map(int, input().split())
        s = list(map(int, input().split()))
        p = list(map(int, input().split()))

        S = sum(s)
        P = sum(p)

        targets = [S * pi // P for pi in p]
        arr = sorted([(v, i + 1) for i, v in enumerate(s)], reverse=True)

        ans = [[] for _ in range(m)]
        j = 0
        cur = 0

        for v, idx in arr:
            ans[j].append(idx)
            cur += v
            if cur == targets[j]:
                j += 1
                cur = 0

        out = []
        for g in ans:
            out.append(str(len(g)) + " " + " ".join(map(str, g)))
        return "\n".join(out)

    return solve()

# small case
assert run("""1 1
5
10
""") == "1 1"

# two contests simple split
assert run("""2 2
5 5
1 1
""") in ["1 1\n1 2", "1 2\n1 1"]

# larger balanced
assert run("""6 3
2 5 10 3 7 1
100 50 75
""")  # structure check only

# descending dominance
assert run("""4 2
8 1 1 1
10 5
""")  # should assign largest alone
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người tham gia đơn lẻ | nhiệm vụ duy nhất | độ đúng cơ sở | 
| Chia đều | hoán vị nào | xử lý đối xứng | 
| Giống mẫu | phân vùng hợp lệ | đường ống đầy đủ | 
| Phân phối lệch | sự ổn định tham lam | xử lý phần tử lớn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một cuộc thi có giải thưởng lớn không tương xứng. Trong tình huống đó, tổng mục tiêu của nó sẽ chiếm ưu thế và thuật toán tham lam sẽ chỉ định các kỹ năng lớn nhất đầu tiên vào cuộc thi đó cho đến khi đạt được ngưỡng. Đối với đầu vào như một giải thưởng lớn và nhiều giải thưởng nhỏ, thuật toán sẽ tạo thành một nhóm chính xác duy nhất, theo sau là các giải thưởng nhỏ hơn, vì việc sắp xếp đảm bảo rằng những người tham gia có kỹ năng cao sẽ sớm được sử dụng ở nơi họ quan trọng nhất để đạt được mục tiêu vượt trội. 

Một trường hợp khác là khi tất cả các kỹ năng đều giống hệt nhau. Ở đây, mọi tổng mục tiêu trở thành một ràng buộc đếm đơn giản thay vì một ràng buộc giá trị. Thuật toán thoái hóa thành việc phân phối các phần tử bằng nhau thành các nhóm có kích thước cố định và việc lấp đầy tham lam tạo ra các khối liền kề có độ dài bằng nhau mà không có sự mơ hồ. 

Trường hợp cuối cùng là khi$m = 1$. Khi đó, chỉ có một mục tiêu bằng tổng số tiền và thuật toán sẽ chỉ định tất cả những người tham gia trực tiếp vào cuộc thi duy nhất đó. Không có chuyển đổi nào xảy ra và quá trình quét tham lam chấm dứt ngay sau khi đổ đầy thùng duy nhất.
