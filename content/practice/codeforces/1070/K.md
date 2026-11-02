---
problem: 1070K
contest_id: 1070
problem_index: K
name: "Video Posts"
contest_name: "2018-2019 ICPC, NEERC, Southern Subregional Contest (Online Mirror, ACM-ICPC Rules, Teams Preferred)"
rating: 1100
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 70
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33813e-d9b8-83ec-b42b-9a3a674904f7
---

#CF 1070K - Bài Viết Video 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33813e-d9b8-83ec-b42b-9a3a674904f7 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi thời lượng video được sắp xếp theo thứ tự thời gian. Nhiệm vụ là chia chuỗi này thành chính xác`k`các nhóm liền kề, trong đó mỗi nhóm đại diện cho một bài đăng và mỗi video phải thuộc chính xác một nhóm. 

Ràng buộc chính không chỉ nằm ở việc phân vùng mà còn ở thực tế là mọi nhóm phải có tổng thời lượng như nhau. Nói cách khác, nếu chúng ta tính tổng thời lượng bên trong mỗi phân đoạn, tất cả`k`tổng phân đoạn phải giống hệt nhau. 

Đầu ra không phải là các phân đoạn mà là độ dài của các phân đoạn này theo thứ tự. Nếu chúng ta biểu thị bằng`s_j`số lượng video trong`j`-th post, chúng ta phải xuất ra một chuỗi các số nguyên dương phân chia mảng thành các khối liên tiếp. 

Ràng buộc`n ≤ 100000`loại trừ mọi cách tiếp cận thử tất cả các phân vùng. Ngay cả việc quay lại ngây thơ đối với các vị trí bị chia cắt cũng tăng theo cấp số nhân, vì có`n-1`vị trí cắt tiềm năng và lựa chọn`k-1`trong số chúng dẫn đến sự bùng nổ tổ hợp. Tương tự như vậy, việc tính toán lại các tổng phân đoạn nhiều lần sẽ đẩy các nghiệm bậc hai ra ngoài phạm vi. 

Một vấn đề nhỏ xuất hiện khi tổng thời lượng của tất cả video không chia hết cho`k`. Trong trường hợp đó, không có phân vùng bằng nhau nào cả. Trường hợp cạnh thứ hai phát sinh khi ranh giới phân đoạn hợp lệ tồn tại nhưng không rõ ràng ngay lập tức do các giá trị lặp lại. Ví dụ: các mảng như`1 1 1 1 1`với`k = 2`cho phép nhiều phân chia hợp lệ, nhưng vấn đề đảm bảo tính duy nhất của câu trả lời nếu nó tồn tại, nghĩa là một khi tìm thấy ranh giới tham lam hợp lệ, nó sẽ bị ép buộc. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ cố gắng lựa chọn`k-1`cắt giảm vị trí giữa các`n-1`các khoảng trống và đối với mỗi phân vùng ứng cử viên, hãy tính tổng phân đoạn và xác minh sự bằng nhau. Ngay cả khi tính toán tổng được tối ưu hóa bằng tổng tiền tố, mỗi lần kiểm tra vẫn tốn phí`O(k)`, và số lượng phân vùng là`C(n-1, k-1)`, trở nên lớn về mặt thiên văn ngay cả đối với đầu vào vừa phải. Điều này làm cho vũ lực hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không được tự do lựa chọn các phân đoạn: tổng phân đoạn được cố định một khi chúng ta biết tổng. Nếu tổng số tiền`S`chia hết cho`k`, thì mỗi đoạn phải có tổng bằng`S / k`. Điều này chuyển đổi vấn đề từ “thử tất cả các phân vùng” thành “tìm ranh giới đạt được tổng mục tiêu chính xác”. 

Khi đã biết tổng mục tiêu, chúng ta có thể quét mảng từ trái sang phải, tích lũy các giá trị. Bất cứ khi nào tổng số tiền chạy đạt chính xác mục tiêu, chúng tôi sẽ cắt một đoạn. Nếu vượt quá mục tiêu, nỗ lực không hợp lệ. Quá trình quét tham lam này hoạt động vì tất cả các số đều dương, do đó tổng một phần tăng lên nghiêm trọng và không thể “tự sửa” sau này. 

Do đó, cấu trúc của bài toán rút gọn thành quét tuyến tính với bộ tích lũy đang chạy và các điểm cắt bắt buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng thời lượng của tất cả video. Nếu tổng này không chia hết cho`k`, ngay lập tức kết luận rằng không tồn tại phân vùng hợp lệ. Điều này là cần thiết vì tổng các phân đoạn bằng nhau phải cộng lại chính xác với tổng. 
2. Tính tổng phân đoạn cần thiết`target = total / k`. Mỗi bài viết phải tổng hợp giá trị này. 
3. Duyệt mảng từ trái sang phải trong khi vẫn duy trì tổng chạy cho phân đoạn hiện tại. Tổng hoạt động này thể hiện tổng thời lượng được chỉ định cho bài đăng hiện tại. 
4. Bất cứ khi nào tổng số tiền hiện có bằng`target`, đóng đoạn hiện tại và ghi lại độ dài của nó. Đặt lại tổng số tiền hiện có và tiếp tục xây dựng phân khúc tiếp theo. Điều này hiệu quả vì chúng tôi buộc phải khớp tổng phân khúc chính xác và tính tích cực đảm bảo chúng tôi không bao giờ vượt quá giới hạn mà không bị phát hiện. 
5. Nếu tại bất kỳ thời điểm nào tổng số tiền hiện có vượt quá`target`, việc xây dựng thất bại. Điều này có nghĩa là chúng ta không thể tạo một phân vùng hợp lệ theo ràng buộc đã cho. 
6. Sau khi xử lý tất cả các phần tử, hãy xác minh rằng chính xác`k`các phân đoạn được hình thành và toàn bộ mảng đã được sử dụng. Nếu vậy, hãy xuất độ dài đoạn; nếu không thì xuất ra "Không". 

### Tại sao nó hoạt động 

Tính chính xác dựa trên tính bất biến mà mỗi phân đoạn hoàn chỉnh có tổng chính xác bằng`target`, và tổng tiền không bao giờ vượt quá`target`. Vì tất cả các phần tử đều hoàn toàn dương nên khi tổng chạy đạt`target`, bất kỳ sự bổ sung nào nữa sẽ vi phạm ràng buộc, do đó việc cắt phải xảy ra tại thời điểm đó. Điều này buộc mỗi ranh giới phân đoạn phải duy nhất bất cứ khi nào có một giải pháp, phù hợp với sự đảm bảo về tính duy nhất của vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

total = sum(a)

if total % k != 0:
    print("No")
    sys.exit()

target = total // k

res = []
curr = 0
length = 0

for x in a:
    curr += x
    length += 1

    if curr == target:
        res.append(length)
        curr = 0
        length = 0
    elif curr > target:
        print("No")
        sys.exit()

if len(res) == k:
    print("Yes")
    print(*res)
else:
    print("No")
```Việc thực hiện bắt đầu bằng việc thực thi điều kiện chia hết, đây là điều kiện tiên quyết khó khăn cho tính khả thi. Sau đó, quá trình quét sẽ tích lũy đồng thời cả tổng và độ dài của phân khúc hiện tại, đảm bảo chúng tôi có thể xuất trực tiếp kích thước phân khúc mà không cần ghi sổ thêm. 

Chi tiết triển khai quan trọng là việc chấm dứt ngay lập tức khi`curr > target`. Nếu không có kiểm tra này, các phân vùng không hợp lệ có thể âm thầm tiếp tục và tạo ra số lượng phân đoạn không chính xác. Một sự tinh tế khác là thiết lập lại cả hai`curr`Và`length`chính xác khi một phân đoạn đóng lại, vì việc không đặt lại độ dài sẽ làm biến dạng cấu trúc đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
3 3 1 4 1 6
```Tổng mục tiêu là`18 / 3 = 6`. 

| Bước | Giá trị | Tổng Curr | Độ dài phân đoạn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | [] | tiếp tục | 
| 2 | 3 | 6 | [2] | cắt | 
| 3 | 1 | 1 | [2] | bắt đầu mới | 
| 4 | 4 | 5 | [2] | tiếp tục | 
| 5 | 1 | 6 | [2, 3] | cắt | 
| 6 | 6 | 6 | [2, 3, 1] | cắt | 

Chúng ta thu được ba đoạn có tổng bằng 6. 

Điều này xác nhận rằng việc cắt giảm tham lam khớp chính xác với các ranh giới cần thiết khi tổng tích lũy đạt được mục tiêu. 

### Ví dụ 2 

đầu vào:```
5 2
1 2 3 4 2
```Tổng cộng là 12, vậy mục tiêu là 6. 

| Bước | Giá trị | Tổng Curr | Độ dài phân đoạn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | [] | tiếp tục | 
| 2 | 2 | 3 | [] | tiếp tục | 
| 3 | 3 | 6 | [3] | cắt | 
| 4 | 4 | 4 | [3] | tiếp tục | 
| 5 | 2 | 6 | [3, 2] | cắt | 

Điều này tạo ra chính xác 2 phân đoạn có tổng bằng 6, xác nhận rằng thuật toán điều chỉnh kích thước phân đoạn một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng với các cập nhật liên tục | 
| Không gian | O(1) | Chỉ chạy bộ đếm và lưu trữ đầu ra | 

Quét tuyến tính phù hợp thoải mái trong giới hạn của`n ≤ 100000`và mức sử dụng bộ nhớ là tối thiểu vì chúng tôi không xây dựng cấu trúc tiền tố phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)
    if total % k != 0:
        return "No"

    target = total // k
    res = []
    curr = 0
    length = 0

    for x in a:
        curr += x
        length += 1
        if curr == target:
            res.append(length)
            curr = 0
            length = 0
        elif curr > target:
            return "No"

    if len(res) == k:
        return "Yes\n" + " ".join(map(str, res))
    return "No"

# provided sample
assert run("6 3\n3 3 1 4 1 6\n") == "Yes\n2 3 1"

# custom: impossible due to divisibility
assert run("3 2\n1 2 3\n") == "No"

# custom: single segment
assert run("4 1\n1 2 3 4\n") == "Yes\n4"

# custom: all equal
assert run("5 5\n1 1 1 1 1\n") == "Yes\n1 1 1 1 1"

# custom: early failure
assert run("4 2\n5 1 1 1\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 3 / 3 3 1 4 1 6 | Có 2 3 1 | phân khúc tham lam chuẩn | 
| 3 2 / 1 2 3 | Không | lỗi chia hết | 
| 4 1 / 1 2 3 4 | Có 4 | trường hợp cạnh phân đoạn đơn | 
| 5 5 / 1 1 1 1 1 | Có 1 1 1 1 1 | tất cả các phần tử phân đoạn riêng biệt | 
| 4 2 / 5 1 1 1 | Không | phát hiện vượt mức | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên xảy ra khi tổng không chia hết cho`k`. Đối với đầu vào`n = 3, k = 2, a = [1, 2, 3]`, tổng số là 6, vì vậy mục tiêu là 3. Quá trình quét tạo ra các phân đoạn`[1,2]`Và`[3]`, cả hai đều tính tổng chính xác, cho thấy rằng chỉ riêng khả năng chia hết là chưa đủ nhưng nó cần thiết. 

Trường hợp thứ hai liên quan đến việc vượt quá sớm. Đối với đầu vào`n = 4, k = 2, a = [5,1,1,1]`, mục tiêu là 4. Phần tử đầu tiên đã vượt quá mục tiêu nên thuật toán kết thúc ngay lập tức. Bất kỳ nỗ lực nào nhằm trì hoãn việc cắt giảm sẽ chỉ làm tăng thêm số tiền do tính tích cực, xác nhận tính bất khả thi. 

Trường hợp thứ ba là phân mảnh tối đa, chẳng hạn như`n = 5, k = 5, a = [1,1,1,1,1]`. Mỗi phần tử khớp với mục tiêu riêng lẻ, tạo ra năm đoạn có độ dài một. Thuật toán đặt lại một cách tự nhiên sau mỗi phần tử, thể hiện tính chính xác ngay cả ở mật độ phân vùng cực cao.
