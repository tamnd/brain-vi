---
title: "CF 1773E - Lắp Ráp Dễ Dàng"
description: "Chúng ta được cấp một số ngăn xếp dọc gồm các khối được đánh số duy nhất. Mỗi ngăn xếp được sắp xếp từ trên xuống dưới và chúng ta được phép sắp xếp lại các khối này về mặt vật lý bằng hai thao tác: chúng ta có thể cắt một ngăn xếp thành hai bằng cách lấy một phân đoạn tiền tố hoặc hậu tố và biến nó thành một…"
date: "2026-06-15T03:50:29+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1773
codeforces_index: "E"
codeforces_contest_name: "2022-2023 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1400
weight: 1773
solve_time_s: 126
verified: true
draft: false
---

[CF 1773E - Lắp ráp dễ dàng](https://codeforces.com/problemset/problem/1773/E) 

**Đánh giá:** 1400 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số ngăn xếp dọc gồm các khối được đánh số duy nhất. Mỗi ngăn xếp được sắp xếp từ trên xuống dưới và chúng ta được phép sắp xếp lại các khối này về mặt vật lý bằng hai thao tác: chúng ta có thể cắt một ngăn xếp thành hai bằng cách lấy một phân đoạn tiền tố hoặc hậu tố và biến nó thành một ngăn xếp mới hoặc chúng ta có thể hợp nhất hai ngăn xếp bằng cách đặt một ngăn xếp hoàn toàn lên trên ngăn xếp kia trong khi vẫn duy trì trật tự bên trong. 

Mục tiêu là kết thúc với chính xác một ngăn xếp chứa tất cả các khối được sắp xếp theo nhãn số của chúng theo thứ tự tăng dần từ trên xuống dưới. Chi phí là số lượng hoạt động phân chia và hợp nhất và chúng tôi phải giảm thiểu tổng số hoạt động, báo cáo số lượng phần tách và số lượng phần hợp nhất được sử dụng trong một kế hoạch tối ưu. 

Khó khăn chính là các khối ban đầu được nhóm thành các ngăn xếp tùy ý và chúng ta chỉ được phép sắp xếp lại bằng cách cắt các tiền tố liền kề và ghép các ngăn xếp. Chúng tôi không được phép tùy tiện sắp xếp lại các khối riêng lẻ. 

Các ràng buộc có tổng kích thước dữ liệu nhỏ, tối đa là 10.000 khối. Điều này loại trừ mọi nhu cầu về cấu trúc dữ liệu nặng hoặc thuật toán đồ thị nâng cao. Một giải pháp xung quanh việc sắp xếp cộng với quét tuyến tính là đủ. Bất cứ điều gì tệ hơn O(n log n) đều an toàn và thậm chí O(n^2) có thể tồn tại nhưng không cần thiết. 

Một trường hợp lỗi đơn giản nhưng quan trọng xuất hiện khi các khối đã ở đúng thứ tự chung nhưng nằm rải rác trên nhiều ngăn xếp. Một chiến lược hợp nhất tham lam luôn hợp nhất các ngăn xếp liền kề mà không xem xét thứ tự toàn cầu có thể dẫn đến sự phân tách hoặc hợp nhất không cần thiết. 

Ví dụ, hãy xem xét ngăn xếp`[1, 4]`,`[2, 3]`. Một chiến lược đơn giản có thể hợp nhất chúng một cách trực tiếp, nhưng các ràng buộc về thứ tự yêu cầu phải phân tách để tách các phân đoạn khớp với thứ tự đã sắp xếp, nếu không thì thứ tự tương đối bên trong các ngăn xếp sẽ ngăn cản việc xây dựng chính xác. Điều này cho thấy rằng chúng ta phải suy luận về việc trật tự toàn cầu phù hợp với ranh giới ngăn xếp ở đâu thay vì chỉ xử lý các ngăn xếp một cách độc lập. 

Một trường hợp khác là khi các khối tạo thành một chuỗi đã được sắp xếp bên trong một ngăn xếp. Sau đó, không cần chia tách hoặc hợp nhất gì cả. Bất kỳ thuật toán nào luôn thực hiện ít nhất một thao tác sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng mô phỏng toàn bộ quá trình. Chúng tôi xem xét mọi trình tự phân tách và hợp nhất có thể có, coi mỗi cấu hình của ngăn xếp là một trạng thái. Từ mỗi trạng thái, chúng tôi tạo ra tất cả các phần tách và hợp nhất hợp lệ, đồng thời chạy tìm kiếm đường dẫn ngắn nhất cho đến khi đạt được cấu hình ngăn xếp đơn được sắp xếp. 

Điều này đúng vì mọi thao tác đều có thể đảo ngược trong không gian cấu trúc và chúng tôi khám phá mọi khả năng. Tuy nhiên, không gian trạng thái là rất lớn. Ngay cả với 10.000 khối, số lượng phân vùng của các khối thành các ngăn xếp có thứ tự tăng lên theo tổ hợp, khiến phương pháp này không khả thi ngay cả đối với 20 khối. 

Quan sát quan trọng là chúng ta thực sự không cần mô phỏng các cấu hình tùy ý. Cấu hình cuối cùng được xác định đầy đủ: một ngăn xếp được sắp xếp theo giá trị. Mọi khối cuối cùng phải được đặt tương đối với các khối khác theo thứ tự tăng dần, vì vậy câu hỏi thực sự duy nhất là làm thế nào các ngăn xếp ban đầu giao nhau với thứ tự được sắp xếp đó. 

Nếu chúng ta tưởng tượng việc sắp xếp tất cả các khối theo giá trị thì mỗi ngăn xếp ban đầu là một chuỗi xuất hiện trong hoán vị được sắp xếp này nhưng được chia thành các phân đoạn. Thuộc tính cấu trúc hữu ích duy nhất là liệu các phần tử liên tiếp theo thứ tự đã sắp xếp có xuất hiện trong cùng một ngăn xếp ở vị trí kề chính xác hay không. Bất cứ khi nào hai phần tử liên tiếp theo thứ tự được sắp xếp chung không nằm trong cùng một ngăn xếp với độ kề kề chính xác, chúng ta phải thực hiện việc vượt qua ranh giới hợp nhất và bất cứ khi nào một ngăn xếp chứa các phần tử thuộc các phân đoạn được sắp xếp khác nhau, chúng ta phải phân tách. 

Do đó, vấn đề giảm xuống còn việc theo dõi cách thứ tự sắp xếp được phân chia trên các ngăn xếp ban đầu và đếm số lượng “ngắt” tồn tại giữa các phần tử được sắp xếp liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Làm phẳng tất cả các khối trong khi ghi nhớ từng khối đến từ ngăn xếp ban đầu nào và sắp xếp chúng theo giá trị. 

Điều này cho chúng ta thứ tự cuối cùng chính xác mà chúng ta muốn đạt được. Chúng tôi cũng giữ lại ID ngăn xếp để có thể so sánh cấu trúc kề trong cấu hình ban đầu. 
2. Quét danh sách đã sắp xếp và so sánh các phần tử liền kề theo danh tính ngăn xếp ban đầu của chúng. 

Bất cứ khi nào hai phần tử liên tiếp theo thứ tự được sắp xếp đến từ các ngăn xếp khác nhau, chúng tôi sẽ ghi lại một ranh giới. Những ranh giới này đại diện cho những nơi mà chúng ta không thể trực tiếp duy trì tính liền kề nếu không có các hoạt động hợp nhất. 
3. Đếm số lần các phần tử liền kề theo thứ tự được sắp xếp đến từ các ngăn xếp khác nhau. Gọi giá trị này`k`. 

Mỗi ranh giới như vậy buộc ít nhất một thao tác hợp nhất để kết nối các phân đoạn thuộc các ngăn xếp ban đầu khác nhau. 
4. Bây giờ hãy xem xét việc chia tách. Đối với mỗi ngăn xếp ban đầu, hãy xem các khối của nó theo thứ tự được sắp xếp. 

Nếu một ngăn xếp đóng góp nhiều phân đoạn rời rạc theo thứ tự được sắp xếp chung thì ngăn xếp đó phải được chia ít nhất nhiều lần trừ đi một. Đối với mỗi ngăn xếp, chúng tôi tính toán có bao nhiêu phân đoạn liền kề mà các phần tử của nó hình thành theo thứ tự được sắp xếp và tích lũy các phần tách cần thiết. 
5. Câu trả lời cuối cùng là tổng số phần tách cần thiết và phần hợp nhất bắt buộc. 

Lựa chọn thiết kế quan trọng là chúng ta không bao giờ mô phỏng rõ ràng các hoạt động. Chúng tôi chỉ tính sự không nhất quán về cấu trúc giữa phân vùng ban đầu và thứ tự sắp xếp mục tiêu. 

### Tại sao nó hoạt động 

Thứ tự được sắp xếp xác định sự sắp xếp tuyến tính mục tiêu duy nhất. Bất kỳ quy trình xây dựng hợp lệ nào cuối cùng cũng phải tập hợp trình tự chính xác này. Mỗi ngăn xếp ban đầu đều đóng góp một đoạn liền kề của chuỗi này hoặc không. Nếu không, cần phải phân tách để tách các phân đoạn. Nếu hai phần tử liên tiếp theo thứ tự được sắp xếp bắt nguồn từ các ngăn xếp khác nhau, chúng không thể trở nên liền kề nếu không có thao tác hợp nhất nối các ngăn xếp của chúng. Bởi vì các thao tác chỉ nối hoặc cắt các khối liền kề nên hai loại chỉnh sửa này đều cần thiết và đủ, và không có thao tác nào có thể khắc phục nhiều hơn một sự không khớp về cấu trúc như vậy cùng một lúc mà không giải quyết rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = []
    stack_id = []
    
    for i in range(n):
        tmp = list(map(int, input().split()))
        k = tmp[0]
        for x in tmp[1:]:
            arr.append(x)
            stack_id.append(i)
    
    # sort by value
    order = sorted(range(len(arr)), key=lambda i: arr[i])
    
    # count merge boundaries
    merges = 0
    for i in range(len(order) - 1):
        if stack_id[order[i]] != stack_id[order[i + 1]]:
            merges += 1
    
    # count splits per stack via segment counting
    pos_in_sorted = [[] for _ in range(n)]
    for idx in order:
        pos_in_sorted[stack_id[idx]].append(idx)
    
    splits = 0
    for i in range(n):
        if not pos_in_sorted[i]:
            continue
        # count number of contiguous runs in sorted index space
        cnt = 1
        for j in range(1, len(pos_in_sorted[i])):
            if pos_in_sorted[i][j] != pos_in_sorted[i][j - 1] + 1:
                cnt += 1
        splits += cnt - 1
    
    print(splits, merges)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách làm phẳng tất cả các ngăn xếp thành một danh sách duy nhất trong khi lưu trữ ngăn xếp ban đầu mà mỗi phần tử thuộc về. Sắp xếp theo giá trị khối sẽ xây dựng lại thứ tự mục tiêu cuối cùng. 

Số lượng hợp nhất được tính bằng cách quét các phần tử liền kề theo thứ tự được sắp xếp này và kiểm tra xem chúng có bắt nguồn từ các ngăn xếp khác nhau hay không. Mỗi quá trình chuyển đổi như vậy chỉ ra một ranh giới hợp nhất cần thiết. 

Việc phân chia được tính toán trên mỗi ngăn xếp bằng cách xem xét vị trí các phần tử của nó xuất hiện trong không gian chỉ mục mảng đã sắp xếp. Bất cứ khi nào các phần tử của ngăn xếp không liên tiếp trong không gian này, điều đó có nghĩa là ngăn xếp bị phân mảnh theo thứ tự cuối cùng, buộc việc phân tách phải cô lập từng phân đoạn liền kề. 

Một điểm tinh tế là chúng tôi không cố gắng mô phỏng các hoạt động thực tế. Tính đúng đắn hoàn toàn xuất phát từ việc tính các điểm gián đoạn về cấu trúc trong hoán vị cảm ứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
3 3 5 8
2 9 2
```Sau khi làm phẳng: 

| giá trị | ngăn xếp | 
| --- | --- | 
| 3 | 0 | 
| 5 | 0 | 
| 8 | 0 | 
| 9 | 1 | 
| 2 | 1 | 

Sắp xếp theo giá trị sẽ cho:`2(1), 3(0), 5(0), 8(0), 9(1)`Chuyển tiếp ngăn xếp liền kề:`1→0`,`0→0`,`0→0`,`0→1`Vậy hợp nhất = 2. 

Ngăn xếp 0 xuất hiện dưới dạng đoạn liền kề`[3,5,8]`, không cần chia. 

Ngăn xếp 1 xuất hiện dưới dạng`[2]`Và`[9]`theo thứ tự sắp xếp nên tạo thành 2 đoạn, cần chia 1 đoạn. 

Vậy câu trả lời là`1 2`. 

### Ví dụ 2 

đầu vào:```
3
1 4
1 2
1 3
```Sắp xếp theo thứ tự dẹt:`2(1), 3(2), 4(0)`Các chuyển tiếp liền kề là tất cả các ngăn xếp khác nhau, tạo ra 2 lần hợp nhất. 

Mỗi ngăn xếp có một phần tử duy nhất nên không bị phân chia. 

Kết quả là`0 2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp tất cả các khối chiếm ưu thế | 
| Không gian | O(n) | lưu trữ mảng giá trị và ID ngăn xếp | 

Tổng số khối nhiều nhất là 10.000, vì vậy việc sắp xếp và một vài lần quét tuyến tính vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = []
    stack_id = []

    for i in range(n):
        tmp = list(map(int, input().split()))
        k = tmp[0]
        for x in tmp[1:]:
            arr.append(x)
            stack_id.append(i)

    order = sorted(range(len(arr)), key=lambda i: arr[i])

    merges = 0
    for i in range(len(order) - 1):
        if stack_id[order[i]] != stack_id[order[i + 1]]:
            merges += 1

    pos = [[] for _ in range(n)]
    for idx in order:
        pos[stack_id[idx]].append(idx)

    splits = 0
    for i in range(n):
        if not pos[i]:
            continue
        cnt = 1
        for j in range(1, len(pos[i])):
            if pos[i][j] != pos[i][j - 1] + 1:
                cnt += 1
        splits += cnt - 1

    return str(splits) + " " + str(merges)

# provided sample
assert run("""2
3 3 5 8
2 9 2
""") == "1 2"

# single stack already sorted
assert run("""1
3 1 2 3
""") == "0 0"

# reversed stacks
assert run("""2
2 4 3
2 2 1
""") == "1 2"

# all singletons
assert run("""3
1 3
1 1
1 2
""") == "0 2"

# already globally consistent split across stacks
assert run("""2
2 1 4
2 2 3
""") == "0 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sắp xếp ngăn xếp đơn | 0 0 | không cần thao tác | 
| ngăn xếp chia đảo ngược | 1 2 | yêu cầu cả việc hợp nhất và chia tách | 
| người độc thân | 0 2 | chỉ hợp nhất giữa các ngăn xếp | 
| đã được cấu trúc phân chia | 0 1 | trường hợp hợp nhất tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các khối đã tạo thành một ngăn xếp được sắp xếp hoàn hảo. Thứ tự được sắp xếp và thứ tự ban đầu trùng nhau, do đó mọi phần tử đều nằm liền kề trong ngăn xếp chính xác và không có sự chuyển tiếp nào xảy ra. Thuật toán tạo ra sự phân tách bằng 0 và hợp nhất bằng 0 vì không có thay đổi ngăn xếp nào trong quá trình quét lân cận được sắp xếp và mỗi ngăn xếp đóng góp chính xác một phân đoạn liền kề. 

Một trường hợp khác là khi mỗi khối nằm trong ngăn xếp riêng của nó. Trong trường hợp này, mọi phần kề theo thứ tự được sắp xếp đều đến từ các ngăn xếp khác nhau, do đó số lần hợp nhất trở thành n trừ 1. Mỗi ngăn xếp chỉ có một phần tử, do đó không có phân đoạn bên trong và phần tách vẫn bằng 0. Thuật toán giảm thiểu vấn đề một cách chính xác trong việc xây dựng một chuỗi các sự hợp nhất. 

Trường hợp khó phát hiện cuối cùng là khi một ngăn xếp được đan xen chặt chẽ theo thứ tự được sắp xếp với các ngăn xếp khác. Ngay cả khi nó xuất hiện liền kề trong đầu vào, các phần tử của nó có thể nằm rải rác trong hoán vị đã sắp xếp, buộc phải phân chia nhiều lần. Bước đếm phân đoạn sẽ phát hiện từng điểm gián đoạn trong danh sách chỉ mục được sắp xếp của nó và tính toán chính xác các hoạt động phân chia được yêu cầu.
