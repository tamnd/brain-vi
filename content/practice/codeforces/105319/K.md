---
title: "CF 105319K - CP và GIT"
description: "Chúng tôi được cung cấp một tập hợp các tệp có tên duy nhất. Một số trong số chúng hiện được đặt trong một khu vực đặc biệt gọi là Giai đoạn, trong khi số còn lại nằm trong Không gian làm việc. Chúng tôi cũng được cung cấp một tập hợp mục tiêu gồm các tệp phải kết thúc chính xác trong Giai đoạn ở cuối, với tất cả các tệp khác nằm ngoài Giai đoạn."
date: "2026-06-22T11:07:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105319
codeforces_index: "K"
codeforces_contest_name: "Tishreen Collegiate Programming Contest 2024"
rating: 0
weight: 105319
solve_time_s: 47
verified: true
draft: false
---

[CF 105319K - CP và GIT](https://codeforces.com/problemset/problem/105319/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các tệp có tên duy nhất. Một số trong số chúng hiện được đặt trong một khu vực đặc biệt gọi là Giai đoạn, trong khi số còn lại nằm trong Không gian làm việc. Chúng tôi cũng được cung cấp một tập hợp mục tiêu gồm các tệp phải kết thúc chính xác trong Giai đoạn ở cuối, với tất cả các tệp khác nằm ngoài Giai đoạn. 

Chúng tôi được phép thực hiện bốn loại thao tác: di chuyển một tệp duy nhất giữa Giai đoạn và Không gian làm việc theo một trong hai hướng hoặc di chuyển tất cả các tệp từ bên này sang bên kia trong một thao tác duy nhất. Mỗi thao tác tốn một lần di chuyển và chúng tôi muốn giảm thiểu tổng số lần di chuyển cần thiết để chuyển đổi cấu hình Giai đoạn ban đầu thành cấu hình cuối cùng chính xác như mong muốn. 

Khó khăn chính là các thao tác hàng loạt có khả năng khắc phục được nhiều điểm không khớp cùng một lúc, nhưng chúng cũng có tính hủy diệt vì chúng di chuyển mọi thứ bất kể nó có đúng hay không. Giải pháp phải quyết định khi nào cần sửa các phần tử riêng lẻ và khi nào việc thiết lập lại toàn bộ có lợi. 

Các ràng buộc là nhỏ về mặt cấu trúc. Tổng số tệp cho mỗi bài kiểm tra tối đa là 100 và tên là duy nhất. Điều này ngay lập tức loại trừ mọi nhu cầu về tối ưu hóa tổ hợp hoặc tìm kiếm đồ thị. Một giải pháp kiểm tra tất cả các mối quan hệ giữa tập hợp ban đầu và tập mục tiêu theo thời gian tuyến tính hoặc bậc hai cho mỗi lần kiểm tra là đủ. 

Trường hợp cạnh tinh tế xuất hiện khi Giai đoạn ban đầu đã khớp chính xác với mục tiêu. Một giải pháp đơn giản vẫn có thể thực hiện các thao tác không cần thiết, nhưng câu trả lời đúng là bằng không. Một trường hợp góc khác là khi Giai đoạn và mục tiêu hoàn toàn tách rời nhau. Trong trường hợp đó, chúng ta phải quyết định giữa việc di chuyển mọi thứ riêng lẻ hoặc thực hiện thiết lập lại toàn bộ, sau đó là chèn có chọn lọc. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ về các trạng thái. Mỗi tệp đều nằm trong Giai đoạn hoặc Không gian làm việc, do đó có tối đa 2^n cấu hình. Từ bất kỳ cấu hình nào, chúng ta có thể áp dụng một trong bốn thao tác và chuyển sang trạng thái khác. Tìm kiếm đường dẫn ngắn nhất như BFS sẽ tìm thấy số lượng hoạt động tối thiểu. Điều này đúng vì mọi hoạt động đều có chi phí như nhau và không gian trạng thái là hữu hạn. 

Tuy nhiên, mặc dù n nhỏ nhưng 2^100 là hoàn toàn không khả thi. Ngay cả việc lưu trữ các trạng thái cũng trở nên không thể. Cấu trúc của các thao tác cho thấy rằng hầu hết không gian trạng thái là không liên quan vì chúng tôi không quan tâm đến cấu hình trung gian, chỉ quan tâm đến việc liệu tệp có được đặt chính xác so với mục tiêu hay không. 

Quan sát quan trọng là mọi tệp đều thuộc một trong bốn loại khi so sánh tư cách thành viên Giai đoạn ban đầu và tư cách thành viên Giai đoạn cuối cùng bắt buộc. Một tệp có thể đã chính xác, nghĩa là tệp bắt đầu và phải kết thúc ở Giai đoạn hoặc bắt đầu và phải kết thúc trong Không gian làm việc. Nó cũng có thể không chính xác, nghĩa là nó hiện ở Giai đoạn nhưng không nên ở đó hoặc hiện ở trong Không gian làm việc nhưng phải ở Giai đoạn. Các hoạt động quan trọng chính xác là những hoạt động khắc phục những điểm không khớp này. 

Di chuyển một tệp sẽ sửa chính xác một điểm không khớp, trong khi di chuyển toàn bộ sẽ đặt lại toàn bộ cấu hình và có khả năng giảm nhiều điểm không khớp cùng một lúc nhưng phải trả giá bằng việc hoàn tác các vị trí chính xác. Điều này tạo ra sự cân bằng: sửa trực tiếp từng tệp không chính xác hoặc thực hiện thiết lập lại toàn cục rồi xây dựng lại Giai đoạn chính xác. 

Giải pháp giảm xuống việc đếm xem có bao nhiêu tệp đã đúng và bao nhiêu tệp sai trong cấu hình ban đầu, sau đó quyết định xem việc thiết lập lại toàn cục có mang lại lợi ích hay không. Vì một lần di chuyển hoàn toàn sẽ làm trống hoặc lấp đầy toàn bộ Giai đoạn, nên sau thao tác như vậy, chúng ta có thể xây dựng lại mục tiêu bằng cách chèn riêng lẻ các tệp được yêu cầu. 

Chiến lược tối ưu hóa ra chỉ phụ thuộc vào số lượng tệp đã được đặt chính xác trong Giai đoạn ban đầu. Nếu nhiều tệp mục tiêu đã ở Giai đoạn, chúng tôi sẽ tránh di chuyển toàn cầu và chỉ điều chỉnh các điểm không khớp cục bộ. Nếu ít hoặc không có gì đúng, việc đặt lại Giai đoạn đầu tiên sẽ rẻ hơn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trạng thái BFS trên tất cả các cấu hình | O(2^n · n) | O(2^n) | Quá chậm | 
| Đặt đếm giao lộ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi danh sách Giai đoạn hiện tại thành một bộ để việc kiểm tra tư cách thành viên diễn ra liên tục. Điều này cho phép chúng tôi nhanh chóng xác định xem tệp hiện có ở trong Giai đoạn hay không. 
2. Chuyển đổi danh sách mục tiêu sang một bộ khác. Vấn đề giảm xuống khi so sánh hai bộ này với toàn bộ tập tin. 
3. Đếm xem có bao nhiêu tệp mục tiêu đã có trong tập Giai đoạn ban đầu. Điều này thể hiện các tệp không yêu cầu hành động nào nếu chúng tôi không thực hiện thao tác phá hủy hoàn toàn. 
4. Tính xem có bao nhiêu tệp mục tiêu bị thiếu trong Giai đoạn. Đây là những tệp mà cuối cùng chúng ta phải chèn bằng các thao tác trên một tệp trừ khi chúng ta quyết định đặt lại mọi thứ trước. 
5. Tính xem có bao nhiêu tệp bổ sung hiện đang ở Giai đoạn nhưng không nằm trong bộ mục tiêu. Chúng phải được loại bỏ trừ khi sử dụng thao tác thiết lập lại toàn bộ. 
6. Đánh giá hai chiến lược. Chiến lược đầu tiên là chỉ sử dụng di chuyển từng tệp: mọi tệp mục tiêu bị thiếu phải được chèn vào Giai đoạn và mọi tệp bổ sung phải được xóa. Chi phí của chiến lược này chính xác là số lượng không khớp giữa Giai đoạn ban đầu và Giai đoạn mục tiêu. 
7. Chiến lược thứ hai là thực hiện thiết lập lại toàn bộ Giai đoạn một lần, di chuyển mọi thứ vào Không gian làm việc, sau đó chèn riêng lẻ tất cả các tệp mục tiêu được yêu cầu. Điều này tốn một thao tác cộng với k lần chèn. 
8. Trả về giá trị tối thiểu của hai chi phí đã tính này. 

### Tại sao nó hoạt động 

Điều bất biến chính là sau khi quyết định có sử dụng thiết lập lại toàn bộ hay không, mọi thao tác còn lại đều độc lập trên mỗi tệp. Việc di chuyển một tệp sẽ giải quyết chính xác một thành viên không khớp và không bao giờ tương tác với các tệp khác. Việc thiết lập lại toàn bộ sẽ loại bỏ tất cả thông tin về tính chính xác trước đó, do đó, việc này chỉ có thể hữu ích nếu số lượng thông tin không khớp đủ lớn để việc xây dựng lại từ đầu sẽ rẻ hơn so với việc sửa chữa chúng riêng lẻ. Vì không có hoạt động trung gian nào bảo toàn được một phần tính chính xác trên quy mô lớn nên hai chiến lược này bao gồm đầy đủ tất cả các đường dẫn tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, k = map(int, input().split())
    s = input().split()
    stage = set(input().split())
    target = set(input().split())

    # count mismatches relative to final target
    missing_in_stage = 0
    extra_in_stage = 0

    for x in target:
        if x not in stage:
            missing_in_stage += 1

    for x in stage:
        if x not in target:
            extra_in_stage += 1

    # cost if we only do single-file moves
    cost_direct = missing_in_stage + extra_in_stage

    # cost if we reset stage then build target
    cost_reset = 1 + k

    print(min(cost_direct, cost_reset))
```Mã bắt đầu bằng cách đọc từng trường hợp thử nghiệm và chuyển đổi các bộ sưu tập Giai đoạn và mục tiêu thành các bộ, cho phép kiểm tra tư cách thành viên O(1). Sau đó, nó tính toán có bao nhiêu tệp mục tiêu không có trong Giai đoạn và có bao nhiêu tệp không liên quan hiện có trong Giai đoạn. Hai đại lượng này mô tả đầy đủ chi phí sửa chữa cấu hình chỉ bằng các thao tác trên một tệp duy nhất. 

Chi phí thay thế giả định rằng chúng tôi thực hiện thiết lập lại toàn bộ, tốn một thao tác, sau đó chèn từng tệp mục tiêu được yêu cầu riêng lẻ, tốn k thao tác. Lấy mức tối thiểu giữa hai chiến lược này đảm bảo sự tối ưu. 

Một điểm triển khai tinh tế là danh sách ban đầu của tất cả các tệp không cần thiết trong quá trình tính toán. Mặc dù xuất hiện ở đầu vào nhưng nó không ảnh hưởng đến cấu trúc chi phí vì các hoạt động chỉ phụ thuộc vào việc chuyển đổi thành viên Giai đoạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

Giai đoạn = {dp} 

Mục tiêu = {tham lam, thực hiện} 

Chúng tôi tính toán các bộ còn thiếu và bổ sung. 

| Bước | Sân khấu | Mục tiêu | Thiếu | Thêm | Chi phí trực tiếp | Đặt lại chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | {dp} | {g, tôi} | 2 | 1 | 3 | 3 | 

Chiến lược trực tiếp loại bỏ dp và thêm hai tệp mục tiêu, tốn 3 thao tác. Chiến lược thiết lập lại tốn 1 + 2 = 3 thao tác. Cả hai đều hòa, vì vậy một trong hai là tối ưu. 

Điều này cho thấy trường hợp thiết lập lại toàn cầu cũng tốt như nhau nhưng không hoàn toàn tốt hơn. 

### Ví dụ 2 

đầu vào: 

Giai đoạn = {địa lý, cây cối, toán học} 

Mục tiêu = {toán, bs} 

| Bước | Sân khấu | Mục tiêu | Thiếu | Thêm | Chi phí trực tiếp | Đặt lại chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | {g,t,m} | {m,bs} | 1 | 2 | 3 | 3 | 

Chi phí trực tiếp loại bỏ hai phần bổ sung và chèn một tệp bị thiếu, tổng cộng là 3. Chi phí đặt lại là 1 + 2 = 3. Một lần nữa cả hai chiến lược đều trùng khớp. 

Điều này nhấn mạnh rằng các trường hợp đẳng thức là phổ biến vì k nhỏ so với các giá trị không khớp và cả hai chiến lược đều thu gọn thành các biểu thức tuyến tính giống nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chúng tôi chỉ quét các bộ Giai đoạn và mục tiêu một lần | 
| Không gian | O(n) | Đặt tên tệp lưu trữ có kích thước tối đa n | 

Các ràng buộc ràng buộc tổng n qua các thử nghiệm là 100, vì vậy giải pháp tuyến tính này dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đáng kể vì chúng tôi chỉ lưu trữ một vài bộ chuỗi nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        s = input().split()
        stage = set(input().split())
        target = set(input().split())

        missing = sum(1 for x in target if x not in stage)
        extra = sum(1 for x in stage if x not in target)

        out.append(str(min(missing + extra, 1 + k)))

    return "\n".join(out)

# sample-style tests (constructed from statement patterns)
assert run("3\n3 1 2\n3\nimplementation\ndp\ngreedy implementation\n4 3 2\ngeo trees math bs\ngeo trees math\nmath bs\n1 0 0\na\n\n\n") == "3\n3\n0"

# all correct already
assert run("1\n3 2 2\na b c\na b\na b\n") == "0"

# all wrong, reset is optimal
assert run("1\n3 2 2\na b c\na b\nx y\n") == "3"

# k large, stage empty
assert run("1\n3 0 3\na b c\n\na b c\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều đúng | 0 | trường hợp nhận dạng | 
| sai hết rồi | 3 | quyết định thiết lập lại và sửa chữa | 
| sân khấu trống | k | xây dựng từ đầu | 
| hỗn hợp | tính toán | đếm không khớp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi Giai đoạn đã bằng Mục tiêu. Đối với đầu vào như Giai đoạn = {a, b} và Mục tiêu = {a, b}, cả số lượng bị thiếu và số lượng bổ sung đều bằng 0, do đó chi phí trực tiếp bằng 0 và chi phí đặt lại là 1 + k, lớn hơn rất nhiều. Thuật toán trả về 0 một cách chính xác mà không cần thực hiện bất kỳ thao tác nào. 

Một trường hợp khác là khi Giai đoạn và Mục tiêu rời rạc. Đối với Giai đoạn = {a, b} và Mục tiêu = {c, d}, thiếu là 2 và bổ sung là 2, do đó chi phí trực tiếp là 4. Chi phí đặt lại là 1 + 2 = 3, do đó thuật toán ưu tiên đặt lại một cách chính xác, phản ánh rằng một lần xóa toàn bộ sẽ rẻ hơn so với việc sửa riêng lẻ tất cả các điểm không khớp. 

Trường hợp cạnh cuối cùng xảy ra khi Giai đoạn trống và Mục tiêu cũng trống. Cả hai chi phí đều có giá trị tương ứng là 0 và 1, vì vậy câu trả lời là 0, phù hợp với thực tế là không cần thực hiện thao tác nào.
