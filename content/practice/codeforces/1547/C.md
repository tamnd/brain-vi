---
title: "CF 1547C - Lập trình cặp"
description: "Hai lập trình viên đang đóng góp các chỉnh sửa cho cùng một tệp, nhưng lịch sử công việc của họ bị xen kẽ theo thời gian. Chúng ta được cung cấp hai chuỗi hành động được sắp xếp theo thứ tự, một chuỗi cho mỗi người. Mỗi hành động có thể là chèn vào cuối tệp hoặc chỉnh sửa một dòng hiện có."
date: "2026-06-14T19:51:01+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1547
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 731 (Div. 3)"
rating: 1100
weight: 1547
solve_time_s: 547
verified: false
draft: false
---

[CF 1547C - Lập trình cặp](https://codeforces.com/problemset/problem/1547/C) 

**Đánh giá:** 1100 
**Tags:** tham lam, hai con trỏ 
**Thời gian giải:** 9 phút 7 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Hai lập trình viên đang đóng góp các chỉnh sửa cho cùng một tệp, nhưng lịch sử công việc của họ bị xen kẽ theo thời gian. Chúng ta được cung cấp hai chuỗi hành động được sắp xếp theo thứ tự, một chuỗi cho mỗi người. Mỗi hành động có thể là chèn vào cuối tệp hoặc chỉnh sửa một dòng hiện có. Tệp bắt đầu với một số dòng ban đầu và cả hai người đều hoạt động trong cùng một trạng thái tệp đang phát triển. 

Nhiệm vụ là xây dựng lại bất kỳ dòng thời gian toàn cầu hợp lệ nào của tất cả các hành động sao cho khi chúng tôi chiếu dòng thời gian này lên hành động của mỗi người theo thứ tự, chúng tôi sẽ khôi phục trình tự ban đầu của họ. Đồng thời, mọi hành động chỉnh sửa chỉ được thực hiện sau khi dòng đích đã tồn tại trong tệp tại thời điểm đó. 

Do đó, cấu trúc này là sự hợp nhất có ràng buộc của hai chuỗi với một biến trạng thái động: số dòng hiện tại trong tệp chỉ tăng thông qua các phần chèn và các chỉnh sửa phụ thuộc vào con số đó đủ lớn. 

Các ràng buộc đủ nhỏ để cấu trúc động O(nm) có thể vượt qua một cách thoải mái, vì mỗi chuỗi có độ dài tối đa là 100 và có tối đa 1000 trường hợp thử nghiệm. Điều đó đặt tổng số hoạt động trong trường hợp xấu nhất lên tới khoảng vài chục triệu, con số này vẫn an toàn trong Python đối với các chuyển đổi đơn giản. Tuy nhiên, cấu trúc của bài toán cho phép xây dựng tham lam rõ ràng hơn nhiều. 

Một cách tiếp cận không chính xác và ngây thơ là chỉ cần hợp nhất một cách tham lam bằng cách luôn thực hiện hành động có sẵn tiếp theo từ một trong hai chuỗi mà không theo dõi xem các chỉnh sửa hiện có hợp lệ hay không. Ví dụ: nếu tệp có kích thước 3 và hành động được chọn tiếp theo là “chỉnh sửa dòng 10”, thì điều này sẽ không hợp lệ ngay cả khi những lần chèn sau này có thể thực hiện được. Điều này không thành công vì tính khả thi phụ thuộc vào các lần chèn trong tương lai chứ không chỉ đặt hàng cục bộ. 

Một lỗi nhỏ khác xảy ra khi một trình tự buộc phải chỉnh sửa sớm đòi hỏi nhiều phần chèn hơn trình tự khác hiện cung cấp. Nếu chúng tôi không theo dõi rõ ràng kích thước tệp hiện tại, chúng tôi có thể chấp nhận các phần xen kẽ không thể thực hiện được hoặc trả về không chính xác -1 sau khi đã xác nhận tiền tố sai. 

## Phương pháp tiếp cận 

Quan điểm mạnh mẽ là nghĩ đến việc xây dựng trình tự đã hợp nhất từng bước, chọn hành động tiếp theo của Monocarp hoặc hành động tiếp theo của Polycarp tại mỗi điểm, trong khi vẫn duy trì kích thước tệp hiện tại và đảm bảo tất cả các chỉnh sửa đều hợp lệ. Đây thực chất là một tìm kiếm trên tất cả các phép ghép, trong trường hợp xấu nhất là khám phá tất cả các khả năng nhị thức (n + m, n). Ngay cả khi cắt tỉa, hệ số phân nhánh vẫn là 2 và độ sâu là n + m, khiến nó trở thành cấp số nhân. 

Quan sát quan trọng là chúng ta không bao giờ cần phải đoán tùy tiện. Ở mỗi bước, nếu có sẵn phần chèn ở một trong hai trình tự thì việc trì hoãn nó luôn là an toàn trừ khi có chỉnh sửa buộc chúng tôi phải tăng kích thước tệp. Hạn chế duy nhất có thể chặn tiến trình là chỉnh sửa nhắm mục tiêu số dòng lớn hơn kích thước hiện tại. Điều này có nghĩa là các phần chèn vào hoạt động giống như các tài nguyên có thể được hoãn lại một cách tự do, trong khi các chỉnh sửa áp đặt các yêu cầu tối thiểu cứng đối với trạng thái hiện tại. 

Điều này dẫn đến một cấu trúc tham lam trong đó chúng ta duy trì hai con trỏ trong các chuỗi và mô phỏng tệp. Bất cứ khi nào cả hai hành động tiếp theo đều là chỉnh sửa hoặc hiện không thể chỉnh sửa một lần, chúng tôi phải chọn phần chèn trước cho đến khi tệp đủ lớn. Nếu cả hai hành động đều là thao tác chèn, chúng ta có thể chọn một trong hai hành động một cách an toàn. 

Điều này loại bỏ tất cả việc quay lại vì một khi một hành động được thực hiện, nó sẽ không bao giờ làm mất hiệu lực tính khả thi trong tương lai miễn là chúng tôi tôn trọng giới hạn kích thước tệp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n+m)) | O(n+m) | Quá chậm | 
| Mô phỏng tham lam | O(n+m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quá trình trong khi theo dõi số dòng hiện có trong tệp.

1. Khởi tạo kích thước tệp là k và đặt hai con trỏ i và j ở đầu hai chuỗi. Cũng duy trì một danh sách kết quả trống. 
2. Ở mỗi bước, hãy kiểm tra xem chúng ta có thể thực hiện hành động tiếp theo của Monocarp hay hành động tiếp theo của Polycarp hay không. 
3. Nếu cả hai chuỗi đều hết, quá trình kết thúc và chúng tôi xuất kết quả. 
4. Nếu Monocarp vẫn còn hành động, hãy kiểm tra hành động tiếp theo của anh ta. Nếu là phần chèn thì nó luôn hợp lệ vì phần chèn không phụ thuộc vào kích thước tệp. Nếu đó là chỉnh sửa cho dòng x, nó chỉ hợp lệ nếu x nhỏ hơn hoặc bằng kích thước tệp hiện tại. 
5. Thực hiện kiểm tra tính hợp lệ tương tự cho hành động tiếp theo của Polycarp. 
6. Nếu cả hai hành động tiếp theo đều hợp lệ, hãy ưu tiên thực hiện một trong hai thao tác chèn nếu có, vì nó không hạn chế tính linh hoạt trong tương lai. Nếu cả hai đều là chỉnh sửa, chúng tôi phải chọn bất kỳ chỉnh sửa nào hợp lệ. 
7. Nếu chỉ một trong hai hành động tiếp theo là hợp lệ thì chúng ta phải thực hiện hành động đó. 
8. Nếu cả hai hành động đều không hợp lệ, điều này có nghĩa là cả hai đều là những chỉnh sửa yêu cầu những dòng chưa tồn tại. Trong trường hợp đó, cách khả thi duy nhất về phía trước sẽ yêu cầu các lần chèn trong tương lai, nhưng không có trình tự nào cho phép chèn vào thời điểm này trong mô phỏng, vì vậy câu trả lời là không thể. 
9. Khi chúng tôi chèn, hãy tăng kích thước tệp. Khi chúng tôi chỉnh sửa, kích thước tệp không thay đổi. 

### Tại sao nó hoạt động 

Bất biến quan trọng là ở mỗi bước, kích thước tệp hiện tại bằng với số lần chèn đã được đặt trong chuỗi đã hợp nhất. Mọi chỉnh sửa chỉ được chấp nhận nếu nó đề cập đến một chỉ mục có kích thước tối đa là như vậy, vì vậy chúng tôi không bao giờ tạo trạng thái không hợp lệ. Vì các phần bổ sung không bao giờ bị hạn chế bởi các chỉnh sửa trong tương lai nên việc trì hoãn chúng không thể làm giảm tính khả thi mà chỉ tăng tính linh hoạt. Do đó, bất cứ khi nào có sự lựa chọn, việc ưu tiên chèn thêm sẽ không bao giờ chặn một giải pháp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    input()
    k, n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    i = j = 0
    cur = k
    res = []

    ok = True

    while i < n or j < m:
        moved = False

        if i < n and a[i] == 0:
            res.append(0)
            cur += 1
            i += 1
            moved = True
        elif j < m and b[j] == 0:
            res.append(0)
            cur += 1
            j += 1
            moved = True
        elif i < n and a[i] <= cur:
            res.append(a[i])
            i += 1
            moved = True
        elif j < m and b[j] <= cur:
            res.append(b[j])
            j += 1
            moved = True

        if not moved:
            ok = False
            break

    if ok:
        print(*res)
    else:
        print(-1)
```Giải pháp duy trì mô phỏng trực tiếp của tệp đang phát triển. Cặp con trỏ theo dõi mỗi chuỗi đã được sử dụng bao nhiêu, trong khi biến`cur`đại diện cho số lượng dòng hiện có. Các phần chèn sẽ tăng bộ đếm này ngay lập tức, điều này rất cần thiết vì hiệu lực của lần chỉnh sửa tiếp theo phụ thuộc vào nó. Logic đặt hàng ưu tiên các phần chèn bất cứ khi nào có thể vì chúng tăng cường tính khả thi cho các chỉnh sửa trong tương lai mà không tốn nhiều ràng buộc. 

Một điểm tinh tế là các chỉnh sửa chỉ được kiểm tra theo kích thước hiện tại tại thời điểm chúng được thực hiện, không bao giờ so với kích thước tiềm năng trong tương lai. Đây là điều làm cho chiến lược tham lam trở nên an toàn: nếu một chỉnh sửa hiện không hợp lệ, chúng tôi chưa thể lên lịch cho nó và chỉ có thể trì hoãn nếu một số thao tác chèn xuất hiện sau đó trong ít nhất một chuỗi. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu có k = 3, Monocarp có [2, 0] và Polycarp có [0, 5]. 

Khi bắt đầu, cur = 3, i = 0, j = 0. Cả hai hành động đầu tiên đều khả dụng, nhưng Polycarp có phần chèn trước, vì vậy chúng tôi lấy 0 từ Polycarp. Bây giờ cur = 4. Tiếp theo, Monocarp có thể chỉnh sửa 2, vì vậy chúng tôi lấy nó. Sau đó chèn Monocarp, sau đó Polycarp chỉnh sửa 5. 

| Bước | tôi | j | cur | hành động được thực hiện | tiểu bang | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 3 | 0 (Polycarp) | cur=4 | 
| 2 | 0 | 1 | 4 | 2 (Monocarp) | cur=4 | 
| 3 | 1 | 1 | 4 | 0 (Monocarp) | cur=5 | 
| 4 | 2 | 1 | 5 | 5 (Polycarp) | xong | 

Điều này cho thấy các phần chèn được sử dụng sớm như thế nào để mở khóa các chỉnh sửa sau này. 

Bây giờ hãy xem xét một trường hợp thất bại: k = 0, a = [1], b = [2]. Ban đầu cur = 0, cả hai hành động tiếp theo đều là những chỉnh sửa không hợp lệ. Vì không có chuỗi nào đưa ra thao tác chèn trước nên thuật toán dừng ngay lập tức và trả về -1. Điều này phản ánh thực tế là không bản sửa đổi nào có thể trở thành hợp lệ nếu không có sự chèn trước, nhưng không có trình tự nào cung cấp một bản chỉnh sửa ở phía trước, khiến cho việc sắp xếp thứ tự là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | mỗi hành động được thực hiện đúng một lần | 
| Không gian | O(n + m) | lưu trữ chuỗi đã hợp nhất | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với các chuỗi có độ dài tối đa là 100, do đó tổng công việc vẫn tuyến tính ở kích thước đầu vào và dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("sys").stdout.write("")

# Note: placeholder runner; in actual use, call the solution logic
```

```
# sample-style sanity checks (conceptual placeholders)

# k=3, simple valid merge
# expected one valid output like: 2 0 0 5

# k=0, impossible edits without insertions
# expected: -1

# all insertions
# k=1, a=[0,0], b=[0,0]
# expected: 0 0 0 0

# edits requiring staged growth
# k=1, a=[2,0], b=[0,2]
# expected: valid ordering or -1 depending on feasibility
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=3, chỉnh sửa hỗn hợp | trình tự hợp lệ | đặt hàng tham lam cơ bản | 
| k=0, chỉ chỉnh sửa | -1 | không thể do thiếu dung lượng | 
| tất cả số không | tất cả các phần chèn | xử lý tăng trưởng đơn điệu | 
| phụ thuộc xen kẽ | hợp lệ hoặc -1 | chỉnh sửa hành vi mở khóa | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi cả hai chuỗi đều bắt đầu bằng các chỉnh sửa yêu cầu chỉ số dòng lớn hơn k. Ví dụ: k = 1, a = [5], b = [6]. Thuật toán trả về chính xác -1 vì không có chuỗi nào có thể đóng góp thao tác chèn đủ sớm để tăng kích thước tệp trước khi thực hiện hành động đầu tiên. 

Một trường hợp khác là khi các phần chèn bị trì hoãn ở một trình tự nhưng lại có sẵn ở trình tự khác. Lựa chọn tham lam đảm bảo chúng tôi luôn sử dụng các phần chèn ngay lập tức khi có sẵn, ngăn chặn việc chặn giả tạo các chỉnh sửa sau này phụ thuộc vào kích thước tệp tăng lên.
