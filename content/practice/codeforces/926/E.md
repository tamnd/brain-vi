---
problem: 926E
contest_id: 926
problem_index: E
name: "Merge Equal Elements"
contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 1900
tags: ["constructive algorithms", "data structures"]
answer: passed_samples
verified: true
solve_time_s: 65
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a3270f4-c654-83ec-aadf-ab79419d4fc3
---

#CF 926E - Hợp nhất các phần tử bằng nhau 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3270f4-c654-83ec-aadf-ab79419d4fc3 

--- 

## Giải pháp 

## Hiểu vấn đề 

Cho ta một dãy số nguyên dương xếp thành một dòng. Quá trình này liên tục tìm kiếm hai số giống hệt nhau nằm cạnh nhau. Bất cứ khi nào một cặp như vậy tồn tại, cặp đó ngoài cùng bên trái sẽ được chọn, cả hai phần tử đều bị xóa và một phần tử bằng giá trị của chúng cộng với một phần tử sẽ được chèn vào vị trí của chúng. Điều này làm thay đổi độ dài chuỗi bằng cách giảm nó đi một và nó cũng có thể tạo các cặp bằng nhau liền kề mới, do đó quá trình tiếp tục cho đến khi không có hai phần tử lân cận nào bằng nhau. 

Nhiệm vụ là tính toán chuỗi ổn định cuối cùng sau khi tất cả các phép hợp nhất có thể được thực hiện. 

Ràng buộc rằng chuỗi có thể có tới 200.000 phần tử ngay lập tức loại trừ mọi giải pháp quét mảng liên tục và thực hiện xóa theo thời gian tuyến tính cho mỗi thao tác. Trong trường hợp xấu nhất, mỗi lần hợp nhất sẽ giảm độ dài đi một, do đó có thể có các thao tác O(n) và một mô phỏng đơn giản cũng quét cặp ngoài cùng bên trái mỗi lần có thể giảm xuống O(n²), tốc độ này quá chậm. 

Một vấn đề tế nhị hơn là việc hợp nhất có thể xếp tầng. Một lần hợp nhất có thể tạo ra một giá trị phù hợp với hàng xóm mới của nó, buộc phải hợp nhất chuỗi nhiều lần. Ví dụ: trong một chuỗi như 1, 1, 2, 2, việc hợp nhất cặp đầu tiên sẽ tạo ra số 2, số 2 này có thể ngay lập tức tạo thành một sự hợp nhất khác. Bất kỳ cách tiếp cận đúng nào cũng phải tính đến các tầng này mà không cần khởi động lại quá trình quét toàn bộ. 

Một trường hợp cạnh khác là khi các giá trị bằng nhau ban đầu không liền kề mà chỉ trở nên liền kề sau nhiều lần hợp nhất. Một chiến lược con trỏ tham lam ngây thơ chỉ nhìn cục bộ và không bao giờ xem lại các vị trí trước đó sẽ thất bại trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ quét mảng liên tục từ trái sang phải để tìm lần xuất hiện đầu tiên của các hàng xóm bằng nhau, thực hiện hợp nhất và sau đó bắt đầu quét lại. Điều này đúng vì nó khớp chính xác với định nghĩa quy trình. Tuy nhiên, mỗi lần quét là O(n) và chúng tôi có thể thực hiện việc hợp nhất O(n), dẫn đến hành vi O(n²) trong trường hợp xấu nhất, chẳng hạn như các phần tử giống hệt nhau chạy trong thời gian dài liên tục bị thu gọn. 

Quan sát quan trọng là quá trình này chỉ sửa đổi cấu trúc cục bộ xung quanh điểm chèn gần đây nhất. Khi chúng tôi xử lý các phần tử từ trái sang phải, mọi khả năng hợp nhất liên quan đến các phần tử trước đó sẽ luôn được kích hoạt bởi giá trị được hình thành gần đây nhất. Điều này gợi ý việc duy trì cấu trúc đại diện cho tiền tố rút gọn hiện tại và chỉ phản ứng cục bộ khi phần tử mới được thêm vào. 

Một ngăn xếp tự nhiên mã hóa ý tưởng này. Chúng tôi xử lý các phần tử từ trái sang phải, đẩy chúng vào một ngăn xếp. Bất cứ khi nào hai phần tử trên cùng bằng nhau, chúng ta hợp nhất chúng thành một phần tử duy nhất có giá trị tăng thêm một. Phần tử mới này có thể lại bằng phần tử trước đó trong ngăn xếp, vì vậy chúng ta phải tiếp tục hợp nhất cho đến khi độ ổn định được khôi phục ở trên cùng. Việc thu gọn cục bộ lặp đi lặp lại này mô phỏng chính xác những gì sẽ xảy ra nếu chúng ta luôn hợp nhất cặp có sẵn ngoài cùng bên trái trong chuỗi ban đầu. 

Sự thay đổi khái niệm quan trọng là mặc dù quy tắc ban đầu đề cập đến cặp ngoài cùng bên trái trên toàn cầu, nhưng mọi hợp nhất chỉ ảnh hưởng đến một vùng liền kề và khi chúng tôi xử lý các phần tử theo thứ tự, vùng hoạt động ngoài cùng bên trái luôn ở trên cùng của biểu diễn ngăn xếp của chúng tôi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Hợp nhất ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một ngăn xếp đại diện cho chuỗi rút gọn hiện tại sau khi xử lý tiền tố của mảng.

1. Bắt đầu với một ngăn xếp trống. Chúng tôi sẽ xử lý đầu vào từ trái sang phải. 
2. Với mỗi phần tử x trong mảng, đẩy x vào ngăn xếp. Điều này thể hiện việc thêm nó vào chuỗi hiện tại. 
3. Sau mỗi lần đẩy, kiểm tra xem hai phần tử cuối cùng trong ngăn xếp có bằng nhau không. Nếu không, thì không thể hợp nhất ở vị trí này, vì vậy chúng ta tiếp tục đến phần tử đầu vào tiếp theo. 
4. Nếu hai phần tử cuối cùng bằng nhau, hãy loại bỏ cả hai và thay thế chúng bằng một phần tử duy nhất bằng giá trị của chúng cộng với một. 
5. Sau khi thực hiện việc thay thế này, phần tử trên cùng mới bây giờ có thể bằng phần tử trước đó trong ngăn xếp, vì vậy chúng tôi lặp lại việc kiểm tra và hợp nhất lại miễn là hai phần tử cuối cùng bằng nhau. 
6. Tiếp tục quá trình này cho đến khi ngăn xếp ổn định ở trên cùng, sau đó tiến hành phần tử đầu vào tiếp theo. 
7. Sau khi tất cả các phần tử được xử lý, ngăn xếp biểu thị chuỗi cuối cùng. 

Việc hợp nhất lặp đi lặp lại ở trên cùng là cần thiết vì mỗi lần hợp nhất có thể tạo ra một "mang" mới đi lên, tương tự như phép cộng trong một hệ thống số nơi các chữ số liền kề giống hệt nhau kết hợp và lan truyền. 

### Tại sao nó hoạt động 

Ngăn xếp duy trì bất biến rằng sau khi xử lý k phần tử đầu tiên của đầu vào, nó thể hiện kết quả chính xác của việc giải quyết đầy đủ tất cả các phép hợp nhất có thể xảy ra hoàn toàn trong tiền tố đó. Bất kỳ sự hợp nhất nào trong toàn bộ quá trình đều phải liên quan đến các phần tử liền kề mà tại thời điểm chúng trở nên bằng nhau, tương ứng với phần trên cùng của cấu trúc này. Vì việc hợp nhất không bao giờ phụ thuộc vào các phần tử trong tương lai và chỉ thu gọn các khối bằng nhau liền kề trở lên nên mọi thao tác hợp lệ trong quy trình ban đầu đều được phản ánh bằng sự thu gọn tương ứng ở đầu ngăn xếp. Điều này đảm bảo rằng không có sự hợp nhất nào bị bỏ sót và không có sự hợp nhất không hợp lệ nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    st = []
    
    for x in a:
        st.append(x)
        
        while len(st) >= 2 and st[-1] == st[-2]:
            x = st[-1]
            st.pop()
            st.pop()
            st.append(x + 1)
    
    print(len(st))
    print(*st)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo quy trình ngăn xếp. Mỗi phần tử được thêm một lần và mọi thao tác hợp nhất sẽ được xử lý ngay lập tức ở đầu ngăn xếp. Việc tái sử dụng biến của`x`đảm bảo rằng sau khi hợp nhất hai giá trị bằng nhau, kết quả tăng dần sẽ được đưa chính xác vào các hợp nhất tiềm năng tiếp theo. 

Một điểm tinh tế là chúng tôi không bao giờ bắt đầu lại quá trình quét từ đầu. Tất cả việc truyền bá cần thiết đều được xử lý bằng cách kiểm tra lặp đi lặp lại ở đầu ngăn xếp, giúp duy trì tính chính xác trong khi vẫn giữ độ phức tạp tuyến tính. 

## Ví dụ đã hoạt động 

Xét dãy 5, 2, 1, 1, 2, 2. 

| Bước | Đầu vào | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | 5 | [5] | 
| 2 | 2 | [5, 2] | 
| 3 | 1 | [5, 2, 1] | 
| 4 | 1 | [5, 2, 2] → hợp nhất → [5, 3] | 
| 5 | 2 | [5, 3, 2] | 
| 6 | 2 | [5, 3, 3] → hợp nhất → [5, 4] | 

Điều này cho thấy cách hợp nhất cục bộ kích hoạt tầng và cách ngăn xếp thu được quá trình nén lặp lại một cách tự nhiên mà không cần quét lại các phần trước đó của chuỗi. 

Bây giờ hãy xem xét 1, 1, 1, 1. 

| Bước | Đầu vào | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | 1 | [1] | 
| 2 | 1 | [2] | 
| 3 | 1 | [2, 1] | 
| 4 | 1 | [2, 2] → [3] | 

Ví dụ này cho thấy một phản ứng dây chuyền đầy đủ trong đó nhiều sự hợp nhất lan truyền lên trên, cuối cùng thu gọn toàn bộ chuỗi thành một giá trị duy nhất. 

Ví dụ thứ hai chứng minh rằng việc sáp nhập không phải là các sự kiện độc lập và các kết quả trung gian phải được xem xét lại nhiều lần ở ranh giới của cấu trúc hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đẩy một lần và xuất hiện tối đa một lần cho mỗi sự kiện hợp nhất và mỗi lần hợp nhất sẽ giảm tổng kích thước ngăn xếp. | 
| Không gian | O(n) | Ngăn xếp lưu trữ hầu hết tất cả các phần tử trong trường hợp xấu nhất trước khi xảy ra bất kỳ sự hợp nhất nào. | 

Hành vi tuyến tính phù hợp thoải mái trong giới hạn 200.000 phần tử, vì mỗi thao tác có thời gian khấu hao không đổi và không thực hiện quét lặp lại. 

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

# provided sample
assert run("6\n5 2 1 1 2 2\n") == "2\n5 4"

# single element
assert run("1\n10\n") == "1\n10"

# no merges
assert run("4\n1 2 3 4\n") == "4\n1 2 3 4"

# full collapse
assert run("4\n1 1 1 1\n") == "1\n3"

# cascading carry chain
assert run("5\n1 1 2 2 3\n") == "2\n3 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 5 2 1 1 2 2 | 2 5 4 | hành vi xếp tầng tiêu chuẩn | 
| 1 10 | 1 10 | xử lý kích thước tối thiểu | 
| 1 2 3 4 | 4 1 2 3 4 | không có trường hợp hợp nhất | 
| 4 1 1 1 1 | 1 3 | hợp nhất xếp tầng lặp đi lặp lại | 
| 5 1 1 2 2 3 | 2 3 3 | tương tác chuỗi hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi việc hợp nhất lan truyền nhiều bước lên trên, tạo ra một chuỗi các giá trị bằng nhau liên tiếp trong ngăn xếp. Ví dụ: trong đầu vào 1, 1, 2, 2, 3, quá trình xử lý tạo ra các trạng thái trung gian trong đó việc hợp nhất 1,1 tạo ra 2, sau đó hợp nhất với các 2 liền kề, v.v. Ngăn xếp xử lý việc này một cách chính xác vì sau mỗi lần hợp nhất, nó sẽ kiểm tra lại cặp trên cùng, đảm bảo rằng mọi đẳng thức mới được tạo sẽ được giải quyết ngay lập tức. 

Một trường hợp tinh vi khác là khi các lần tăng lặp lại làm cho các giá trị cách xa nhau trong mảng ban đầu trở nên bằng nhau sau nhiều lần hợp nhất. Vì thuật toán không bao giờ loại bỏ thông tin lịch sử vượt quá ranh giới ngăn xếp nên tất cả các tương tác tiềm năng như vậy sẽ được ghi lại một cách tự nhiên khi các giá trị đó trở nên liền kề trong cấu trúc rút gọn.
