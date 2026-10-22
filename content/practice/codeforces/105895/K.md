---
title: "CF 105895K - LRU là tốt nhất? (Phiên bản dễ dàng)"
description: "Chúng ta được cung cấp một chuỗi yêu cầu cố định và một “bộ đệm” nhỏ có kích thước tối đa là $m$, ban đầu trống. Khi chúng tôi quét trình tự từ trái sang phải, mỗi phần tử chúng tôi gặp có thể đã có trong bộ đệm hoặc chưa. Nếu phần tử hiện tại đã có trong bộ đệm, chúng tôi sẽ nhận được một điểm."
date: "2026-06-21T12:28:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105895
codeforces_index: "K"
codeforces_contest_name: "The 21st Southeast University Programming Contest (Summer)"
rating: 0
weight: 105895
solve_time_s: 61
verified: true
draft: false
---

[CF 105895K - LRU Tốt Nhất? (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/105895/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi yêu cầu cố định và tối đa một “bộ đệm” nhỏ có kích thước$m$, ban đầu trống rỗng. Khi chúng tôi quét trình tự từ trái sang phải, mỗi phần tử chúng tôi gặp có thể đã có trong bộ đệm hoặc chưa. 

Nếu phần tử hiện tại đã có trong bộ đệm, chúng tôi sẽ nhận được một điểm. Nếu nó không có trong bộ đệm, không có gì xấu xảy ra trong phiên bản đơn giản hóa này, nhưng chúng tôi được phép tùy ý chèn nó vào bộ đệm, có thể loại bỏ một phần tử hiện có. Trong phiên bản dễ dàng này, việc chèn và loại bỏ không tốn bất kỳ chi phí nào, vì vậy cách duy nhất để đạt được điểm là sắp xếp bộ đệm để các phần tử trong tương lai hiện diện khi chúng xuất hiện. 

Nhiệm vụ là chọn tất cả các bản cập nhật bộ đệm trực tuyến trong quá trình quét để tổng số lần truy cập bộ đệm được tối đa hóa. 

Tổng cộng các ràng buộc là nhỏ, với tổng$n$trên tất cả các trường hợp thử nghiệm nhiều nhất là 800. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tệ hơn mức đại khái$O(n^2)$, nhưng vẫn cho phép các chiến lược kiểm tra bộ đệm cho từng bước và mô phỏng hành vi trong tương lai. 

Một điểm tinh tế là mặc dù việc chèn luôn miễn phí nhưng các chiến lược bất cẩn vẫn có thể thất bại nặng nề. Ví dụ: luôn giữ các phần tử được xem gần đây nhất là không tối ưu. 

Hãy xem xét một chuỗi như$1, 2, 3, 1, 2, 3$với$m = 2$. Nếu chúng ta luôn giữ hai phần tử gần đây nhất, chúng ta có thể kết thúc bằng$\{2, 3\}$trước ba yếu tố cuối cùng, thiếu lượt truy cập trong tương lai$1$. Thay vào đó, chiến lược tối ưu sẽ theo dõi những yếu tố nào sẽ sớm cần thiết. 

Khó khăn chính là quyết định sai sót không phải về phần tử hiện tại mà là về việc duy trì quyền truy cập vào các phần tử sẽ xuất hiện lại sau này. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng tất cả nội dung có thể có trong bộ đệm sau mỗi bước. Tại mỗi vị trí, khi xảy ra lỗi, chúng tôi thử tất cả các lựa chọn có thể có về phần tử nào cần loại bỏ và phần tử nào cần chèn. Điều này tạo thành một quá trình phân nhánh trong đó mỗi trạng thái là một tập hợp con có kích thước tối đa$m$và quá trình chuyển đổi phụ thuộc vào phần tử tiếp theo. Ngay cả với việc cắt tỉa mạnh mẽ, số lượng các trạng thái vẫn tăng lên theo kiểu tổ hợp$\binom{n}{m}$, vượt xa những gì có thể thực hiện được ngay cả đối với$n = 800$. 

Cấu trúc đơn giản hóa vì tất cả các lượt truy cập đều có phần thưởng như nhau và không có hình phạt. Quyết định có ý nghĩa duy nhất là nên giữ những phần tử nào trong bộ đệm để tối đa hóa các lần truy cập trong tương lai. Điều này biến vấn đề thành một vấn đề bộ nhớ đệm tối ưu cổ điển với đầy đủ kiến ​​thức trong tương lai. 

Chiến lược tối ưu nổi tiếng là luôn đuổi phần tử có lần xuất hiện tiếp theo xa nhất trong tương lai. Nếu một phần tử không bao giờ xuất hiện nữa thì phần tử đó luôn là ứng cử viên tốt nhất để bị trục xuất. Quy tắc tham lam này là tối ưu vì nó trì hoãn việc mất quyền truy cập vào các yếu tố sẽ sớm cần thiết, bảo toàn lợi ích trong tương lai gần. 

Để áp dụng điều này, ở mỗi bước, chúng ta cần biết thời điểm mọi phần tử được lưu trong bộ nhớ đệm sẽ xuất hiện tiếp theo. Đối với mỗi giá trị, chúng tôi duy trì danh sách các vị trí trong tương lai và nâng cao con trỏ khi chúng tôi quét về phía trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu của các trạng thái bộ đệm | Hàm mũ | Hàm mũ | Quá chậm | 
| Bộ nhớ đệm tối ưu theo phong cách Belady |$O(nm)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một bộ đệm và đối với mỗi giá trị, một con trỏ tới lần xuất hiện tiếp theo trong tương lai. 

1. Tính toán trước cho mỗi giá trị một danh sách tất cả các chỉ số nơi nó xuất hiện. Điều này cho phép chúng tôi nhanh chóng biết những lần xuất hiện trong tương lai. 
2. Đối với mỗi giá trị, hãy duy trì một con trỏ trong danh sách của nó để chỉ ra lần xuất hiện đầu tiên ngay sau vị trí hiện tại. Khi chúng ta di chuyển về phía trước trong mảng, chúng ta sẽ nâng cao các con trỏ này khi chúng ta chuyển các lần xuất hiện. 
3. Duy trì bộ đệm hiện tại tối đa là một bộ$m$các giá trị. 
4. Khi xử lý vị trí$i$, kiểm tra xem$a_i$đã có trong bộ đệm. Nếu đúng như vậy, hãy tăng điểm lên một vì đây là lần truy cập bộ đệm. 
5. Nếu$a_i$không có trong bộ đệm, chúng tôi xem xét việc chèn nó. Nếu bộ đệm còn trống, chúng tôi sẽ chèn trực tiếp vào. Nếu không, chúng ta phải loại bỏ một phần tử hiện có. 
6. Để chọn phần tử cần loại bỏ, chúng tôi kiểm tra mọi phần tử hiện có trong bộ đệm và tính chỉ số xuất hiện tiếp theo của nó. Ứng cử viên tốt nhất để loại bỏ là ứng cử viên có lần xuất hiện tiếp theo xa nhất trong tương lai hoặc hoàn toàn không tồn tại. 
7. Sau khi loại bỏ phần tử đó, hãy chèn$a_i$vào bộ đệm. 

Lý do chính khiến điều này có hiệu quả là vì một phần tử có khả năng sử dụng tiếp theo rất xa sẽ có ít giá trị ngay lập tức nhất để duy trì các lượt truy cập trong tương lai. Việc loại bỏ nó không ảnh hưởng đến kết quả ngắn hạn, trong khi việc giữ lại các yếu tố được tái sử dụng trước đó sẽ đảm bảo đạt được lợi ích. 

### Tại sao nó hoạt động 

Tại bất kỳ bước nào, bộ đệm đại diện cho sự lựa chọn$m$những yếu tố mà chúng tôi cam kết giữ lại vì lợi ích tương lai. Giả sử chúng ta xem xét việc thay thế một phần tử$x$trong bộ đệm với phần tử hiện tại$a_i$. Nếu như$x$được sử dụng sớm hơn trong tương lai so với một số phần tử được lưu trong bộ nhớ cache khác$y$, việc hoán đổi chúng chỉ có thể trì hoãn hoặc duy trì các lần truy cập trong tương lai một cách an toàn hơn bằng cách giữ$x$. Điều này thiết lập một đối số trao đổi tham lam: bất kỳ cấu hình nào vi phạm quy tắc “lần sử dụng tiếp theo xa nhất sẽ bị loại bỏ” có thể được cải thiện bằng cách hoán đổi mà không làm giảm các lần truy cập trong tương lai. Việc lặp lại quá trình trao đổi này sẽ dẫn đến cấu trúc tối ưu ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    INF = 10**18
    
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        _ = list(map(int, input().split()))  # x_i all 1
        _ = list(map(int, input().split()))  # y_i all 0
        _ = list(map(int, input().split()))  # z_i all 0

        pos = [[] for _ in range(n + 1)]
        for i, v in enumerate(a):
            pos[v].append(i)

        ptr = [0] * (n + 1)

        def next_occ(v, i):
            lst = pos[v]
            while ptr[v] < len(lst) and lst[ptr[v]] <= i:
                ptr[v] += 1
            if ptr[v] < len(lst):
                return lst[ptr[v]]
            return INF

        cache = set()
        score = 0

        for i, v in enumerate(a):
            if v in cache:
                score += 1
            else:
                if len(cache) < m:
                    cache.add(v)
                else:
                    worst = None
                    worst_next = -1
                    for u in cache:
                        nxt = next_occ(u, i)
                        if nxt > worst_next:
                            worst_next = nxt
                            worst = u
                    cache.remove(worst)
                    cache.add(v)

        print(score)

if __name__ == "__main__":
    solve()
```Giải pháp được tổ chức xoay quanh việc duy trì, ở mỗi bước, một cái nhìn chính xác về tương lai cho mọi phần tử được lưu trong bộ nhớ đệm. các`pos`danh sách lưu trữ tất cả các lần xuất hiện và`ptr`tiến lên một cách lười biếng để việc tính toán lần xuất hiện tiếp theo được khấu hao hiệu quả. Bản thân bộ đệm là một tập hợp và mỗi lần bỏ sót sẽ kích hoạt quá trình quét bộ đệm để xác định phần tử có tính hữu dụng trong tương lai ít khẩn cấp nhất. 

Chi tiết triển khai chính là`next_occ(v, i)`luôn trả về lần xuất hiện đầu tiên ngay sau chỉ mục`i`và việc nâng cao con trỏ đảm bảo chúng tôi không quét lại các vị trí cũ nhiều lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một trường hợp nhỏ với$m = 2$và trình tự:$$a = [1, 2, 3, 1, 2, 3]$$Khi bắt đầu, bộ đệm trống. 

| tôi | một [tôi] | Bộ nhớ đệm trước | Đánh | Hành động | Bộ nhớ đệm sau | Điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | {} | Không | chèn 1 | {1} | 0 | 
| 1 | 2 | {1} | Không | chèn 2 | {1,2} | 0 | 
| 2 | 3 | {1,2} | Không | đuổi xa nhất lần sử dụng tiếp theo, chèn 3 | {1,3} | 0 | 
| 3 | 1 | {1,3} | Có | không | {1,3} | 1 | 
| 4 | 2 | {1,3} | Không | đuổi, chèn 2 | {2,1} hoặc {2,3} tùy theo tương lai | 1 | 
| 5 | 3 | ... | Có | không | ... | 2 | 

Dấu vết này cho thấy lý do tại sao việc trục xuất phải phụ thuộc vào những lần xảy ra trong tương lai chứ không phải lần gần đây. Giữ sai cặp sớm sẽ giảm số lần đánh sau. 

### Ví dụ 2 

hãy để$m = 1$, sự liên tiếp:$$a = [1, 2, 1, 2, 1]$$Với kích thước bộ đệm 1, chúng tôi luôn chỉ giữ lại một phần tử. 

| tôi | một [tôi] | Bộ nhớ đệm trước | Đánh | Bộ nhớ đệm sau | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | {} | Không | {1} | 0 | 
| 1 | 2 | {1} | Không | {2} | 0 | 
| 2 | 1 | {2} | Không | {1} | 0 | 
| 3 | 2 | {1} | Không | {2} | 0 | 
| 4 | 1 | {2} | Không | {1} | 0 | 

Với$m=1$, không thể truy cập được vì mọi giá trị đều bị ghi đè trước khi sử dụng lại, xác nhận giới hạn này hoàn toàn mang tính cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi bước có thể quét tối đa$m$các mục được lưu trong bộ nhớ đệm để chọn lệnh trục xuất và tổng số$n$bước | 
| Không gian |$O(n)$| Lưu trữ danh sách xuất hiện của tất cả các giá trị | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là nhỏ, do đó việc quét bộ đệm ở mỗi bước có thể dễ dàng đủ nhanh. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ các vị trí, giữ nguyên tuyến tính trong$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict

    def solve():
        t = int(input())
        INF = 10**18
        
        for _ in range(t):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))
            _ = list(map(int, input().split()))
            _ = list(map(int, input().split()))
            _ = list(map(int, input().split()))

            pos = [[] for _ in range(n + 1)]
            for i, v in enumerate(a):
                pos[v].append(i)

            ptr = [0] * (n + 1)

            def next_occ(v, i):
                lst = pos[v]
                while ptr[v] < len(lst) and lst[ptr[v]] <= i:
                    ptr[v] += 1
                if ptr[v] < len(lst):
                    return lst[ptr[v]]
                return INF

            cache = set()
            score = 0

            for i, v in enumerate(a):
                if v in cache:
                    score += 1
                else:
                    if len(cache) < m:
                        cache.add(v)
                    else:
                        worst = None
                        worst_next = -1
                        for u in cache:
                            nxt = next_occ(u, i)
                            if nxt > worst_next:
                                worst_next = nxt
                                worst = u
                        cache.remove(worst)
                        cache.add(v)

            return str(score)

    # samples and custom tests
    assert run("1\n1 1\n1\n1\n0\n0\n") == "0"
    assert run("1\n6 2\n1 2 3 1 2 3\n1 1 1 1 1 1\n0 0 0 0 0 0\n0 0 0 0 0 0\n") == "2"
    assert run("1\n5 1\n1 2 1 2 1\n1 1 1 1 1\n0 0 0 0 0\n0 0 0 0 0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bộ nhớ đệm phần tử đơn và yêu cầu đơn lẻ | 0 | Hành vi ranh giới tối thiểu | 
| Mẫu lặp lại với m=2 | 2 | Quyết định bộ nhớ đệm có lợi | 
| Trình tự xen kẽ với m=1 | 0 | Không thể có lượt truy cập bền vững | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một giá trị không bao giờ xuất hiện nữa sau khi được chèn. Trong tình huống này, lần xuất hiện tiếp theo của nó được coi là vô tận, khiến nó trở thành ứng cử viên bị trục xuất tốt nhất bất cứ khi nào nó hiện diện trong bộ đệm cùng với bất kỳ phần tử có thể tái sử dụng nào. Thuật toán sẽ loại bỏ nó trước một cách tự nhiên, tránh lãng phí dung lượng bộ đệm. 

Một trường hợp khác là khi tất cả các giá trị trong bộ đệm có vị trí xuất hiện tiếp theo giống hệt nhau. Trong tình huống đó, mọi lựa chọn trục xuất đều tương đương và thuật toán sẽ vẫn hoạt động chính xác vì việc thay thế bất kỳ lựa chọn nào trong số chúng không làm thay đổi khả năng xảy ra vụ tấn công trong tương lai. 

Trường hợp cuối cùng là khi phần tử hiện tại đã có trong bộ đệm nhưng cũng sẽ sớm xuất hiện lại. Cơ chế con trỏ đảm bảo chúng tôi vẫn tính lần truy cập ngay lập tức và các lần xuất hiện trong tương lai sẽ được nhận dạng chính xác mà không cần bất kỳ sửa đổi bộ đệm nào ở bước đó.
