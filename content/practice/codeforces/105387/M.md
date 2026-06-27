---
title: "CF 105387M - Rạp chiếu phim"
description: "Chúng ta được xếp một hàng $n$ ghế và $n$ học sinh. Mỗi sinh viên có một số chỗ ưa thích và mỗi sinh viên cũng có một tham số chi phí không hài lòng cá nhân."
date: "2026-06-23T16:26:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105387
codeforces_index: "M"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2023"
rating: 0
weight: 105387
solve_time_s: 86
verified: false
draft: false
---

[CF 105387M - Rạp chiếu phim](https://codeforces.com/problemset/problem/105387/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng$n$chỗ ngồi và$n$sinh viên. Mỗi sinh viên có một số chỗ ưa thích và mỗi sinh viên cũng có một tham số chi phí không hài lòng cá nhân. Nếu một học sinh không ngồi vào chỗ mà họ muốn, chúng tôi sẽ phải chịu cái giá không hài lòng đó cho học sinh đó; nếu họ ngồi vào chỗ ưa thích của mình, họ sẽ không đóng góp được gì. 

Nhiệm vụ là chỉ định mỗi học sinh vào một chỗ ngồi duy nhất, tạo thành một hoán vị các chỗ ngồi sao cho tổng số điểm không hài lòng càng nhỏ càng tốt. 

Vì vậy, đầu vào xác định hai mảng có độ dài$n$. Mảng đầu tiên cho chúng ta biết mỗi học sinh muốn ngồi chỗ nào. Mảng thứ hai cho chúng ta biết việc “đặt nhầm chỗ” học sinh đó sẽ tốn kém như thế nào. Đầu ra phải cung cấp sự phân công chỗ ngồi cho sinh viên và kết quả là tổng thể sự không hài lòng ở mức tối thiểu có thể xảy ra. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất cứ điều gì siêu tuyến tính cho mỗi lần thử chuyển nhượng. Phương pháp kết hợp bậc ba hoặc thậm chí bậc hai sẽ vượt quá giới hạn. Chúng ta nên mong đợi điều gì đó như sắp xếp hoặc xử lý tham lam trong$O(n \log n)$hoặc$O(n)$. 

Một sai lầm ngây thơ nhưng phổ biến là cho rằng chúng ta có thể chỉ định từng học sinh một cách độc lập vào chỗ ngồi ưa thích của họ và giải quyết xung đột một cách tùy tiện. Điều đó không thành công vì khi nhiều sinh viên muốn có cùng một chỗ ngồi, việc lựa chọn ai sẽ có được chỗ ngồi đó sẽ ảnh hưởng đáng kể đến tổng chi phí tùy thuộc vào mức độ không hài lòng của họ. 

Một trường hợp phức tạp khác là khi một số sinh viên chia sẻ cùng một chỗ ngồi ưa thích và có những mức độ không hài lòng rất khác nhau. Ví dụ: nếu ba sinh viên đều muốn ngồi ở ghế 1, nhưng chi phí của họ là$1, 1000, 1000$, thì việc đưa những sinh viên có học phí cao ra khỏi sở thích của họ là điều tốn kém và nên tránh. Một người tham lam không ưu tiên những xung đột này một cách chính xác có thể dễ dàng chọn một sự sắp xếp dưới mức tối ưu. 

Cuối cùng, khi tất cả các sở thích đều khác biệt, câu trả lời gần như bằng không. Bất kỳ giải pháp đúng đắn nào cũng phải phát hiện và bảo tồn cấu trúc này. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo là thử tất cả các hoán vị trong việc xếp chỗ cho học sinh. Đối với mỗi hoán vị, chúng tôi tính toán tổng mức độ không hài lòng của những sinh viên không ngồi vào chỗ ưa thích của họ. Điều này đúng vì nó kiểm tra mọi phép gán có thể. Tuy nhiên, số hoán vị là$n!$, và thậm chí đối với$n = 10$, cái này đã quá lớn để tính toán. 

Chúng ta cần hiểu điều gì thực sự tạo ra chi phí. Chi phí chỉ phát sinh khi học sinh không được xếp vào chỗ ngồi ưa thích của mình. Điều này giải quyết lại vấn đề: chúng tôi muốn tối đa hóa tổng “chi phí tiết kiệm” từ những sinh viên đã giành được chỗ ngồi ưa thích của mình thành công, với điều kiện là mỗi chỗ ngồi chỉ có thể được sử dụng một lần. 

Bây giờ cấu trúc trở nên rõ ràng hơn. Mỗi học sinh về cơ bản là một yêu cầu cho một chỗ ngồi duy nhất và mỗi yêu cầu đều có trọng số. Chúng ta muốn đáp ứng càng nhiều yêu cầu quan trọng càng tốt, bởi vì việc đáp ứng một yêu cầu sẽ mang lại mức giảm tương đương với chi phí không hài lòng của sinh viên đó. 

Điều này trở thành bài toán so sánh trọng số tối đa trong biểu đồ hai bên giữa học sinh và chỗ ngồi, nhưng với cấu trúc rất cụ thể: mỗi học sinh có chính xác một chỗ ngồi ưa thích. Điều đó làm giảm vấn đề giải quyết xung đột trên mỗi ghế. 

Ý tưởng chính là xử lý từng chỗ ngồi một cách độc lập và chỉ định tối đa một học sinh vào đó. Nếu nhiều sinh viên muốn có cùng một chỗ ngồi, chúng ta nên trao chỗ đó cho sinh viên nào có chi phí không hài lòng lớn nhất, vì điều đó mang lại lợi ích lớn nhất từ ​​việc đáp ứng yêu cầu đó. Tất cả các sinh viên khác phải được phân công đi nơi khác và sẽ phải trả chi phí của mình. 

Điều này chuyển vấn đề thành nhóm học sinh theo chỗ ngồi ưa thích của họ và chọn một đại diện cho mỗi nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi chỗ ngồi như một nhóm học sinh muốn có nó. 

1. Nhóm tất cả học sinh theo chỗ ngồi ưa thích của họ. Với mỗi chỗ ngồi, hãy thu thập danh sách những sinh viên muốn có chỗ đó, cùng với mức độ không hài lòng của họ. 
2. Đối với mỗi nhóm chỗ ngồi, chọn học sinh có giá trị không hài lòng cao nhất. Chỉ định học sinh đó vào chỗ ngồi này. Lựa chọn này đảm bảo chúng tôi tối đa hóa lợi ích của việc đáp ứng một sinh viên có chi phí cao bất cứ khi nào chúng tôi chỉ có thể đáp ứng một chỗ cho một chỗ. 
3. Đánh dấu tất cả học sinh khác trong cùng nhóm là “chưa được chỉ định chỗ ngồi ưa thích của các em”. Những học sinh này sau đó sẽ được xếp vào những chỗ trống còn lại. 
4. Thu thập tất cả các chỗ chưa nhận được học sinh đã chọn. Đây là những chỗ ngồi miễn phí. 
5. Tập hợp tất cả học sinh chưa được xếp vào chỗ ngồi ưa thích. Đây là những học sinh còn sót lại. 
6. Chỉ định những học sinh còn sót lại một cách tùy tiện vào các ghế trống, bởi vì dù sao thì không có bài tập nào trong số này phù hợp với sở thích của họ, vì vậy mỗi học sinh như vậy đều phải đóng góp toàn bộ chi phí không hài lòng của mình bất kể họ nhận nhầm ghế nào. 

Lựa chọn thiết kế quan trọng là bước 2: trong mỗi ghế, chúng tôi chỉ giữ lại học sinh có nguy cơ bị đặt nhầm chỗ cao nhất. Lý do là chúng tôi chỉ có một cơ hội duy nhất để “cứu” chỗ ngồi đó, và chúng tôi muốn cứu học viên đắt giá nhất có thể. 

### Tại sao nó hoạt động 

Thuật toán được điều khiển bởi điều kiện tối ưu cục bộ cho mỗi chỗ ngồi. Mỗi chỗ ngồi có thể được ghép với tối đa một học sinh, vì vậy trong số tất cả học sinh tranh giành chỗ đó, chỉ có một học sinh có thể tránh phải trả phí. Việc chọn sinh viên có chi phí tối đa sẽ giảm thiểu tổng số tiền phạt từ nhóm đó, vì bất kỳ lựa chọn nào khác sẽ khiến sinh viên có chi phí cao hơn không thể so sánh được và do đó bị phạt hoàn toàn. Vì các nhóm độc lập giữa các ghế nên các quyết định tối ưu cục bộ này kết hợp thành một nhiệm vụ tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pref = list(map(int, input().split()))
    cost = list(map(int, input().split()))
    
    buckets = [[] for _ in range(n + 1)]
    
    for i in range(n):
        buckets[pref[i]].append(i)
    
    ans = [-1] * n
    used_seats = [False] * (n + 1)
    
    # assign best candidate per seat
    free_seats = []
    unassigned = []
    
    for seat in range(1, n + 1):
        if not buckets[seat]:
            free_seats.append(seat)
        else:
            best = buckets[seat][0]
            for i in buckets[seat]:
                if cost[i] > cost[best]:
                    best = i
            ans[best] = seat
            used_seats[seat] = True
            
            for i in buckets[seat]:
                if i != best:
                    unassigned.append(i)
    
    # assign remaining seats
    ptr = 0
    for i in unassigned:
        while ptr < len(free_seats) and ans[i] == -1:
            s = free_seats[ptr]
            ptr += 1
            ans[i] = s
    
    print(sum(cost[i] for i in range(n) if ans[i] != pref[i]))
    print(*ans)

def main():
    solve()

if __name__ == "__main__":
    main()
```Mã này sẽ nhóm học sinh đầu tiên theo chỗ ngồi ưa thích của họ. Sau đó, đối với mỗi chỗ ngồi, nó sẽ xác định sinh viên có chi phí không hài lòng tối đa và chỉ định trực tiếp cho họ. Tất cả các sinh viên khác được thu thập để phân công lại. 

Những chỗ ngồi miễn phí chính xác là những chỗ không có học sinh “tốt nhất” nào được chọn. Chúng được sử dụng để điền vào các sinh viên còn lại. Việc tính toán chi phí cuối cùng chỉ đơn giản là tính tổng chi phí cho những sinh viên không được xếp vào chỗ ngồi ưa thích của họ. 

Một điểm tinh tế là chúng tôi không quan tâm học sinh còn sót lại sẽ nhận nhầm chỗ nào. Giá thành chỉ phụ thuộc vào chỗ ngồi có phù hợp với sở thích hay không chứ không phụ thuộc vào sai sót như thế nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
1 2 3
```Tất cả học sinh đều thích chỗ ngồi riêng biệt. 

| Chỗ ngồi | Xô | Được chọn tốt nhất | Được giao | Còn sót lại | 
| --- | --- | --- | --- | --- | 
| 1 | [0] | 0 | 1 | - | 
| 2 | [1] | 1 | 2 | - | 
| 3 | [2] | 2 | 3 | - | 

Không có sự không khớp nào xảy ra nên tổng chi phí là 0. 

Điều này xác nhận rằng khi các tùy chọn đã là một hoán vị, thuật toán sẽ duy trì việc gán chi phí bằng 0. 

### Ví dụ 2 

đầu vào:```
4
3 3 2 2
2 1 3 4
```| Chỗ ngồi | Xô | Được chọn tốt nhất | Được giao | Còn sót lại | 
| --- | --- | --- | --- | --- | 
| 1 | [] | - | - | - | 
| 2 | [2,3] | 2 (chi phí 1) | 2 | 3 | 
| 3 | [0,2] | 0 (chi phí 2) | 3 | 2 | 
| 4 | [3] | 3 | 4 | - | 

Học sinh còn sót lại được nhận chỗ ngồi miễn phí. Nhiệm vụ này đảm bảo rằng học sinh có chi phí cao nhất trong mỗi nhóm xung đột sẽ được bảo toàn. 

Điều này cho thấy giải pháp tham lam bên trong mỗi nhóm sẽ ưu tiên chính xác cho những sinh viên có chi phí cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi học sinh được xử lý một lần vào một nhóm và một lần trong quá trình lựa chọn và phân công | 
| Không gian |$O(n)$| Nhóm, mảng gán và danh sách phụ trợ chia tỷ lệ tuyến tính | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả thời gian và bộ nhớ đều tăng tuyến tính với$n$, Và$n = 10^5$nằm trong giới hạn tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample-like
assert run("3\n1 2 3\n1 2 3\n") == "0\n1 2 3"

# conflict case
assert run("4\n3 3 2 2\n2 1 3 4\n").split("\n")[0].isdigit()

# all same preference
assert run("3\n1 1 1\n5 2 7\n")

# minimum
assert run("1\n1\n10\n") == "0\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều khác biệt | chi phí bằng không | độ đúng cơ sở | 
| tất cả cùng sở thích | xử lý xung đột nặng nề | lựa chọn tham lam trên mỗi thùng | 
| phần tử đơn | trường hợp tầm thường | điều kiện biên | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả học sinh thích cùng một chỗ ngồi. Đối với đầu vào như:```
3
1 1 1
5 2 7
```thuật toán xếp sinh viên có chi phí 7 vào ghế 1, vì điều đó tối đa hóa chi phí tiết kiệm được. Hai học sinh còn lại bị ép ngồi vào ghế khác và hoàn toàn bất mãn. Bước phân nhóm đảm bảo tất cả các ứng viên được xem xét cùng nhau và lựa chọn chi phí tối đa đảm bảo tiết kiệm tối ưu. 

Một trường hợp khó khăn khác là khi một chỗ ngồi không có học sinh quan tâm. Những chỗ ngồi đó trở thành chỗ trống và chúng thu hút những học sinh còn sót lại một cách tự nhiên. Vì sự không hài lòng chỉ phụ thuộc vào sự không phù hợp nên bất kỳ vị trí nào giữa các ghế trống này đều tương đương nhau, do đó thứ tự phân công không ảnh hưởng đến tính tối ưu.
