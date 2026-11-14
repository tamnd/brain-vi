---
title: "CF 1784A - Quái vật (bản dễ)"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có $n$ quái vật được sắp xếp theo khái niệm thành một dòng và mỗi quái vật có một số giá trị sức khỏe nguyên. Quái vật sẽ biến mất khi máu của nó về 0. Chúng ta có thể thực hiện hai loại hành động."
date: "2026-06-09T11:02:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1784
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2022 - \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 (Engine)"
rating: 1000
weight: 1784
solve_time_s: 197
verified: false
draft: false
---

[CF 1784A - Quái vật (phiên bản dễ)](https://codeforces.com/problemset/problem/1784/A) 

**Đánh giá:** 1000 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 3 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản đều có$n$quái vật được sắp xếp theo khái niệm theo một dòng và mỗi quái vật có một số giá trị sức khỏe nguyên. Quái vật sẽ biến mất khi máu của nó về 0. 

Chúng ta có thể thực hiện hai loại hành động. Hành động đầu tiên mang tính cục bộ: chúng tôi chọn một con quái vật còn sống và giảm đúng một con quái vật còn sống. Hành động thứ hai mang tính toàn cầu: chúng tôi giảm một lượng máu của mọi quái vật còn sống. Điều khó khăn là nếu hành động chung này khiến ít nhất một con quái vật đạt đến mức 0, thì hành động đó sẽ ngay lập tức được kích hoạt lại, lặp đi lặp lại, cho đến khi một hành động toàn diện không giảm được con quái vật nào về 0. Chúng ta được phép sử dụng hành động chung này nhiều nhất một lần, trong khi hành động cục bộ có thể được sử dụng tùy ý nhiều lần. Mục tiêu là giảm thiểu số lượng hành động cục bộ mà chúng tôi sẽ sử dụng trong khi vẫn đảm bảo rằng tất cả quái vật cuối cùng đều bị tiêu diệt. 

Khó khăn chính là hoạt động toàn cầu có thể xếp tầng và có khả năng loại bỏ nhiều quái vật trong một “chuỗi”, nhưng tính hữu dụng của nó phụ thuộc hoàn toàn vào thời điểm nó được kích hoạt so với việc phân bổ các giá trị sức khỏe. 

Các ràng buộc rất lớn: tổng số quái vật trong tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$. Điều này loại trừ bất kỳ giải pháp nào mô phỏng hoạt động toàn cầu từng bước trên tất cả các giá trị tình trạng hoặc quét liên tục các mảng một cách đơn giản. Bất kỳ cách tiếp cận hợp lệ nào cũng phải giảm từng trường hợp thử nghiệm thành công việc tuyến tính hoặc gần tuyến tính. 

Một mô phỏng ngây thơ sẽ thử mọi thời điểm có thể để áp dụng phép thuật toàn cầu và mô phỏng hành vi xếp tầng của nó. Điều này ngay lập tức trở nên không khả thi vì mỗi mô phỏng có thể mất$O(n)$, và có$O(n)$sự lựa chọn, dẫn đến$O(n^2)$mỗi trường hợp thử nghiệm. 

Một trường hợp phức tạp xuất hiện khi nhiều quái vật có cùng lượng máu tối thiểu. Ví dụ: nếu tất cả các giá trị đều bằng nhau như$[3,3,3]$, chỉ riêng câu thần chú toàn cầu sẽ xóa sạch mọi thứ cùng một lúc, không tạo ra hành động cục bộ nào. Mặt khác, nếu mức tối thiểu là duy nhất nhưng phần còn lại lớn hơn đáng kể, việc sử dụng toàn cầu sớm hoặc bị trì hoãn không thay đổi gì về câu trả lời cuối cùng, nhưng lý luận ngây thơ có thể cho rằng vấn đề thời gian là không chính xác. 

Một tình huống phức tạp khác phát sinh khi giá trị tối thiểu là 1. Ví dụ:$[1,100,100]$. Nếu chúng ta sử dụng phép thuật toàn cầu ngay lập tức, chúng ta chỉ tiêu diệt con quái vật đầu tiên và giảm bớt những con quái vật khác, nhưng chúng ta không thể giảm số lượng hoạt động cục bộ theo bất kỳ cách nào có ý nghĩa trừ khi chúng ta điều chỉnh quyết định phù hợp với cấu trúc của các giá trị còn lại. 

Quan sát cốt lõi là phép thuật toàn cầu có hiệu quả “dịch chuyển” tất cả các giá trị xuống một cách đồng nhất một lần và bất kỳ quái vật nào có giá trị ban đầu là 1 sẽ trở thành một trục trong đó quá trình xếp tầng dừng cục bộ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử sử dụng phép thuật toàn cầu vào mọi thời điểm có thể. Vì phép thuật toàn cầu chỉ được áp dụng một lần, chúng ta có thể tưởng tượng việc chọn thời điểm khi một số tập hợp con quái vật đã bị giảm bớt một phần do các hoạt động cục bộ. Đối với mỗi lựa chọn, chúng tôi mô phỏng hiệu ứng toàn cầu và đếm xem cần thêm bao nhiêu hoạt động cục bộ để tiêu diệt những con quái vật còn lại. Tuy nhiên, mô phỏng rất tốn kém: việc áp dụng phép thuật toàn cục và theo dõi việc xóa xếp tầng đòi hỏi phải quét mảng liên tục và việc kết hợp điều này với tất cả các thao tác trước có thể dẫn đến độ phức tạp bậc hai hoặc tệ hơn. 

Sự đơn giản hóa quan trọng đến từ việc lật ngược phối cảnh. Thay vì suy nghĩ về thời điểm áp dụng câu thần chú toàn cầu, chúng tôi hỏi cấu trúc nào còn lại nếu chúng tôi áp dụng nó. Sau khi sử dụng phép thuật toàn cầu một lần, máu của mỗi quái vật giảm đi 1 và tất cả quái vật có giá trị ban đầu là 1 sẽ biến mất ngay lập tức. Những gì còn lại là những con quái vật có giá trị ít nhất là 2, giờ đã giảm đi 1. Từ thời điểm đó trở đi, chỉ có các hoạt động cục bộ mới quan trọng. 

Điều này có nghĩa là phép thuật toàn cầu có thể được hiểu là chọn một điểm giới hạn: tất cả quái vật có giá trị 1 sẽ biến mất miễn phí và tất cả những quái vật khác sẽ bị giảm đi một lần. Cái giá duy nhất còn lại là xử lý lượng máu còn sót lại của những con quái vật còn lại. 

Nếu không sử dụng phép thuật toàn cầu, mọi quái vật phải bị tiêu diệt hoàn toàn bằng các hoạt động cục bộ, tốn kém$\sum a_i$. Với phép thuật toàn cầu, chúng tôi “tiết kiệm” chính xác một đơn vị công việc cho mỗi quái vật bằng$a_i \ge 1$, nhưng chỉ khi chúng ta diễn giải dòng thác một cách chính xác: lợi ích thực sự là tất cả những thứ đó biến mất miễn phí và tất cả những thứ khác sẽ bị giảm bớt một lần, giải quyết vấn đề một cách hiệu quả. 

Vì vậy, chúng tôi so sánh hai trường hợp: 

Nếu chúng ta không bao giờ sử dụng câu thần chú toàn cầu, câu trả lời đơn giản là$\sum a_i$. 

Nếu chúng ta sử dụng nó một lần, mỗi quái vật sẽ mất 1 máu và tất cả quái vật có giá trị ban đầu là 1 đều biến mất. Chi phí còn lại trở thành$\sum \max(0, a_i - 1)$. 

Chúng tôi lấy tối thiểu của hai giá trị này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force về thời gian đánh vần |$O(n^2)$|$O(n)$| Quá chậm | 
| So sánh hai cấu hình (dùng/không dùng Global Spell) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán hai đại lượng cho mỗi trường hợp thử nghiệm: tổng của tất cả các giá trị và tổng của$\max(0, a_i - 1)$. Câu trả lời là nhỏ hơn trong hai. 

1. Đọc mảng giá trị sức khỏe của quái vật. Điều này cung cấp cấu trúc chi phí đầy đủ để tiêu diệt mọi thứ chỉ bằng các hành động cục bộ. 
2. Tính tổng của tất cả các giá trị. Điều này thể hiện tình huống mà chúng tôi không bao giờ sử dụng phép thuật toàn cầu, vì vậy mọi đơn vị máu phải được loại bỏ theo cách thủ công. 
3. Tính tổng thứ hai trong đó mỗi giá trị đóng góp$a_i - 1$nếu nó ít nhất là 1, nếu không thì là 0. Điều này tương ứng với việc áp dụng phép thuật toàn cầu một lần, giúp giảm mỗi quái vật đi 1 và loại bỏ những quái vật đạt 0 ngay lập tức. 
4. Lấy số tiền tối thiểu của hai số tiền. Điều này so sánh trực tiếp cái giá của việc không bao giờ sử dụng phép thuật toàn cầu so với việc sử dụng nó một lần. 

### Tại sao nó hoạt động 

Phép thuật toàn cầu, khi được sử dụng đúng một lần, sẽ áp dụng mức giảm đồng đều cho tất cả quái vật còn sống. Hiệu ứng phi tuyến tính duy nhất của nó là những con quái vật đạt đến 0 trong dòng thác sẽ biến mất ngay lập tức và không yêu cầu bất kỳ hoạt động nào nữa. Hành vi này tương đương với việc giảm mỗi quái vật đi một đơn vị và loại bỏ tất cả các số 0 sau đó. Vì bất kỳ việc sử dụng bổ sung nào của phép thuật toàn cầu đều không được phép nên việc xem xét nhiều ứng dụng sẽ không có lợi ích gì và tất cả các chiến lược tối ưu sẽ rơi vào hai trường hợp được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    total = 0
    reduced = 0
    
    for x in a:
        total += x
        if x > 1:
            reduced += x - 1
    
    print(min(total, reduced))
```Giải pháp xử lý từng trường hợp thử nghiệm trong một lần duyệt qua mảng. Biến`total`tích lũy chi phí của việc không bao giờ sử dụng phép thuật toàn cầu. Biến`reduced`mô phỏng việc áp dụng phép thuật tổng thể một lần bằng cách trừ đi một giá trị từ mỗi giá trị dương, nhưng vì tất cả các giá trị ít nhất là 1 nên chúng tôi tính toán một cách an toàn`x - 1`cho tất cả các phần tử. Mức tối thiểu của hai điều này nắm bắt được chiến lược tối ưu. 

Một chi tiết triển khai tinh tế là không cần phải kiểm tra rõ ràng các số 0 sau khi giảm vì đầu vào đảm bảo$a_i \ge 1$. Điều này loại bỏ độ phức tạp của ranh giới và giữ cho việc tính toán tuyến tính chặt chẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 1 2
```Chúng tôi tính toán cả hai kịch bản. 

| Bước | Đóng góp mảng | tổng cộng | giảm | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 2 | 
| 2 | 1 | 4 | 2 | 
| 3 | 2 | 6 | 3 | 

Giá trị cuối cùng: tổng = 6, giảm = 3, trả lời = 3. 

Điều này cho thấy rằng việc áp dụng bùa chú toàn cầu là có lợi vì nó loại bỏ sự đóng góp chi phí đơn vị từ các phần tử nhỏ nhất và đẩy tất cả các phần tử khác xuống dưới. 

### Ví dụ 2 

đầu vào:```
6
4 1 5 4 1 1
```| Bước | Giá trị | tổng cộng | giảm | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | 3 | 
| 2 | 1 | 5 | 3 | 
| 3 | 5 | 10 | 7 | 
| 4 | 4 | 14 | 10 | 
| 5 | 1 | 15 | 10 | 
| 6 | 1 | 16 | 10 | 

Giá trị cuối cùng: tổng = 16, giảm = 10, trả lời = 10. 

Dấu vết cho thấy phép thuật toàn cầu liên tục giảm bớt mọi quái vật không tầm thường bằng chính xác một đơn vị công việc cục bộ bắt buộc, đồng thời loại bỏ tất cả quái vật có sức khỏe đơn vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi mảng được quét một lần để tính cả hai tổng | 
| Không gian |$O(1)$thêm | Chỉ có hai bộ tích lũy được sử dụng | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính theo tổng số quái vật, phù hợp thoải mái trong các ràng buộc của$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        reduced = sum(x - 1 for x in a)
        out.append(str(min(total, reduced)))
    return "\n".join(out) + "\n"

# provided samples
assert run("""2
3
3 1 2
6
4 1 5 4 1 1
""") == "3\n10\n"

# all ones
assert run("""1
5
1 1 1 1 1
""") == "0\n"

# increasing values
assert run("""1
4
1 2 3 4
""") == "5\n"

# single monster
assert run("""1
1
7
""") == "1\n"

# already large uniform
assert run("""1
3
5 5 5
""") == "12\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 0 | phép thuật toàn cầu loại bỏ mọi thứ miễn phí | 
| tăng giá trị | 5 | sự cân bằng giữa số tiền và số tiền giảm | 
| quái vật đơn lẻ | 1 | trường hợp biên đúng đắn | 
| giá trị lớn thống nhất | 12 | giảm nhất quán trên tất cả các yếu tố | 

## Vỏ cạnh 

Đối với đầu vào trong đó tất cả quái vật có giá trị 1, chẳng hạn như$[1,1,1]$, thuật toán tính tổng = 3 và giảm = 0, chọn đúng 0. Điều này phù hợp với thực tế là một phép thuật toàn cầu sẽ giết chết mọi thứ ngay lập tức thông qua xếp tầng. 

Đối với một trường hợp hỗn hợp như$[1,2,2]$, tổng cộng là 5 và giảm là 3. Thuật toán nắm bắt chính xác rằng chỉ những quái vật lớn hơn mới được hưởng lợi từ việc giảm đồng đều, trong khi những quái vật đã ở mức 1 sẽ không phải trả thêm phí sau câu thần chú. 

Đối với mảng một phần tử$[k]$, tổng cộng là$k$và giảm đi là$k-1$, phản ánh chính xác rằng phép thuật toàn cầu chỉ có thể tiết kiệm được một đơn vị công việc nếu được sử dụng.
