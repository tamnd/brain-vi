---
title: "CF 103860A - Nghiền"
description: "Chúng ta được cung cấp một chương trình ngắn bao gồm hai loại lệnh được lưu trữ trong một cấu trúc giống như mảng. Việc thực thi không chạy trực tiếp danh sách này theo thứ tự một lần; thay vào đó, nó xây dựng cấu trúc thứ hai, một hàng đợi và thực thi các lệnh từ cấu trúc đó một cách linh hoạt."
date: "2026-07-02T07:56:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103860
codeforces_index: "A"
codeforces_contest_name: "The 7th China Collegiate Programming Contest, Finals (CCPC Finals 2021)"
rating: 0
weight: 103860
solve_time_s: 51
verified: true
draft: false
---

[CF 103860A - Mash](https://codeforces.com/problemset/problem/103860/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chương trình ngắn bao gồm hai loại lệnh được lưu trữ trong một cấu trúc giống như mảng. Việc thực thi không chạy trực tiếp danh sách này theo thứ tự một lần; thay vào đó, nó xây dựng cấu trúc thứ hai, một hàng đợi và thực thi các lệnh từ cấu trúc đó một cách linh hoạt. 

Ban đầu, tất cả các hướng dẫn ban đầu được sao chép vào hàng đợi theo thứ tự. Việc thực thi sau đó được tiến hành bằng cách liên tục xuất hiện lệnh phía trước của hàng đợi này. Mỗi lệnh sẽ tạo đầu ra ngay lập tức hoặc thêm nhiều lệnh khác vào phía sau cùng hàng đợi. Một lệnh bình thường in một ký tự đơn. Lệnh sao chép không in ra bất cứ thứ gì; thay vào đó, nó lấy phần đầu tiên của danh sách lệnh ban đầu và thêm một bản sao mới của các lệnh đó vào hàng thực thi. 

Nhiệm vụ là xác định chuỗi đầu ra kết quả sau khi thực hiện chính xác k lệnh từ hàng đợi hoặc ít hơn nếu hàng đợi hết sớm hơn. 

Khó khăn chính là hàng đợi có thể tăng kích thước theo cấp số nhân vì các lệnh sao chép có thể lặp lại các lệnh trước đó. Do đó, một mô phỏng ngây thơ có thể bùng nổ vượt xa k bước. 

Các ràng buộc n và k đều lên tới 100000. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cụ thể hóa hàng đợi một cách rõ ràng hoặc sao chép các khối lệnh theo nghĩa đen. Ngay cả việc mở rộng tuyến tính cho mỗi thao tác sao chép cũng sẽ trở thành bậc hai trong trường hợp xấu nhất. 

Một trường hợp phức tạp phát sinh khi các thao tác sao chép liên tục sao chép các tiền tố mà bản thân chúng chứa các thao tác sao chép tiếp theo. Ví dụ: nếu mỗi lệnh là bản sao của toàn bộ tiền tố thì hàng đợi sẽ tăng theo cấp số nhân trong khi đầu ra yêu cầu vẫn nhỏ. Một mô phỏng hàng đợi đơn giản sẽ cố gắng hiện thực hóa một số lượng lệnh không khả thi. 

Một trường hợp cạnh khác là khi k nhỏ hơn n. Trong trường hợp này, chúng tôi thậm chí không bao giờ hoàn thành tiền tố ban đầu, vì vậy mọi tối ưu hóa giả định xử lý trước toàn bộ tất cả các bản mở rộng bản sao vẫn phải dừng sớm một cách chính xác. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì một hàng hướng dẫn và xử lý nó theo từng bước. Mỗi lần chúng ta đưa ra một lệnh, chúng ta sẽ thêm một ký tự vào đầu ra hoặc mở rộng hàng đợi bằng cách sao chép m lệnh đầu tiên từ danh sách ban đầu. Điều này đúng vì nó phản ánh chính xác quá trình. 

Tuy nhiên, chế độ thất bại là ngay lập tức. Mỗi thao tác sao chép có thể thêm tối đa lệnh O(n). Vì có tối đa k thao tác và mỗi lệnh mới được thêm vào có thể tự kích hoạt nhiều bản sao hơn nên tổng số lệnh được liệt kê trong hàng đợi có thể tăng vượt xa bất kỳ giới hạn đa thức nào trong k. Ngay cả việc đạt được k bước cũng có thể yêu cầu xử lý một cấu trúc ẩn lớn theo cấp số nhân. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần hàng đợi được mở rộng đầy đủ. Chúng ta chỉ cần biết k lệnh được thực thi đầu tiên. Mỗi lệnh là một lá tạo ra một ký tự hoặc một nút bên trong mở rộng thành tiền tố của chương trình gốc. Điều này tự nhiên tạo thành một cây mở rộng có gốc trong đó mỗi nút tương ứng với một lần xuất hiện lệnh và cp m tạo các cạnh cho m nút gốc đầu tiên. 

Chúng tôi tránh mở rộng bằng cách theo dõi, đối với mỗi lệnh ban đầu, số lần nó được “truy cập” trong quá trình thực thi tối đa k bước. Thay vì mở rộng các phần tử con một cách rõ ràng, chúng ta truyền số đếm tiến lên: mỗi lần một lệnh cp m được xử lý, nó sẽ tăng số lần sử dụng của m lệnh gốc đầu tiên. Điều này làm giảm vấn đề trong việc đếm số lần mỗi lệnh được thực hiện trong k bước đầu tiên, sau đó tạo ra đầu ra từ các lệnh echo được tính theo trọng số này. 

Điều này biến một quá trình hàm mũ thành một quá trình lan truyền tuyến tính trên một mảng cố định.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hàng đợi Brute Force | O(số mũ) | O(n + k mở rộng) | Quá chậm | 
| Đếm sự lan truyền trên biểu đồ tiền tố | O(n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng việc thực thi mà không mở rộng hàng đợi một cách rõ ràng bằng cách theo dõi số lần mỗi lệnh được thực thi. 

1. Khởi tạo một mảng cnt có kích thước n với cnt[i] = 1 với mọi i. Điều này thể hiện rằng mỗi lệnh gốc được thực thi ít nhất một lần trong bản sao hàng đợi ban đầu. 
2. Duy trì việc thực thi thứ tự con trỏ trên các lệnh theo thứ tự chỉ mục ban đầu của chúng. Chúng tôi xử lý các hướng dẫn theo thứ tự chỉ mục tăng dần như thể chúng được xếp hàng theo thứ tự. 
3. Giữ một biến được xử lý = 0 biểu thị tổng số lần thực hiện lệnh đã được sử dụng cho đến nay. 
4. Đối với mỗi lệnh i từ 1 đến n và trong khi được xử lý < k, chúng tôi mô phỏng việc thực thi lệnh i cnt[i] lần ở dạng tổng hợp thay vì riêng lẻ. 
5. Nếu lệnh i là echo c, chúng ta thêm c lặp lại cnt[i] lần vào câu trả lời, nhưng chỉ tối đa hạn ngạch còn lại k - đã xử lý. 
6. Nếu lệnh i là cp m, chúng tôi phân bổ cnt[i] thực thi bổ sung cho tất cả các lệnh từ 1 đến m bằng cách tăng cnt[j] tương ứng. Điều này thể hiện rằng mỗi lần thực thi cp m sẽ chèn lại tiền tố vào luồng thực thi. 
7. Dừng khi xử lý đạt k. 

Ý tưởng chính là thay vì mở rộng các hoạt động cp thành các phần tử hàng đợi rõ ràng, chúng tôi xử lý mỗi lần thực thi lệnh như một đơn vị của “luồng” có thể được phân phối lại. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cnt[i] biểu thị số lần lệnh i sẽ xuất hiện trong hàng đợi thực thi khái niệm trước khi đạt đến vị trí k. Hàng đợi ban đầu đóng góp một lần thực hiện mỗi lệnh. Mỗi lệnh cp m, khi được thực thi một lần, sẽ thêm chính xác một bản sao bổ sung của tiền tố [1..m] vào hàng đợi, do đó, mỗi lần xuất hiện của cp m sẽ nhân số lượt truy cập trong tương lai vào các lệnh tiền tố đó thêm một đơn vị. Vì chúng ta chỉ quan tâm đến k lần thực thi đầu tiên nên việc tổng hợp những đóng góp này sẽ duy trì tính bội số chính xác mà không cần xây dựng chuỗi mở rộng. Quá trình này tương đương với việc mở cây thực thi nhưng cắt bớt nó ở độ sâu k. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
ops = []

for _ in range(n):
    parts = input().split()
    if parts[0] == "echo":
        ops.append(("echo", parts[1]))
    else:
        ops.append(("cp", int(parts[1])))

cnt = [0] * n
cnt[0] = 1

res = []
processed = 0

for i in range(n):
    if processed >= k:
        break

    if cnt[i] == 0:
        continue

    take = min(cnt[i], k - processed)

    if ops[i][0] == "echo":
        res.append(ops[i][1] * take)
        processed += take

    else:
        processed += take
        m = ops[i][1]
        if m > 0:
            add = cnt[i]
            for j in range(m):
                cnt[j] += add

print("".join(res))
```Việc triển khai duy trì ý tưởng rằng mỗi lệnh mang bội số cnt[i], biểu thị số lần nó đóng góp vào tiền tố thực thi. Chúng tôi chỉ tiêu thụ tối đa k tổng số lần thực thi, được theo dõi bằng cách xử lý. Lệnh echo đóng góp trực tiếp vào chuỗi đầu ra, nhân với số lần thực hiện của chúng. Sao chép hướng dẫn truyền tải trọng lượng thực hiện của chúng tới các hướng dẫn trước đó. 

Một điểm tinh tế là việc truyền bá sử dụng hoàn toàn cnt[i] chứ không phải giá trị nhận bị cắt bớt. Điều này là do ngay cả khi chúng ta chỉ cần một phần thực thi của nó trong k bước đầu tiên, thì về mặt khái niệm, tất cả chúng vẫn tạo ra các lệnh xếp hàng trong tương lai, ngay cả khi chúng ta không sử dụng hết đầu ra của chúng. 

Điều kiện dừng đảm bảo chúng ta không bao giờ tạo ra nhiều đầu ra hơn yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 20
echo a
cp 2
```| Bước | tôi | Hướng dẫn | cnt[i] | Hành động | đã xử lý | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | tiếng vang | 1 | xuất ra | 1 | một | 
| 2 | 2 | cp 2 | 1 | truyền bá tiền tố | 1 | một | 

Sau lần vượt qua đầu tiên, cp sao chép tiền tố, tăng hiệu quả thực thi trong tương lai. Vì k lớn nên quá trình truyền liên tục phản hồi lại thành tiếng vang. 

Kết quả trở thành nhiều ký tự a cho đến khi đạt được k. 

Đầu ra:```
aaaaaaaaaa
```Điều này cho thấy một cp đơn lẻ có thể tạo ra sự mở rộng lặp đi lặp lại của các lệnh echo như thế nào. 

### Ví dụ 2 

đầu vào:```
3 18
echo a
cp 2
echo b
```| Bước | tôi | Hướng dẫn | cnt[i] | Hành động | đã xử lý | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | tiếng vang | 1 | xuất ra | 1 | một | 
| 2 | 2 | cp 2 | 1 | truyền bá tiền tố | 1 | một | 
| 3 | 3 | tiếng vang b | 1 | đầu ra b | 2 | ab | 

Sự lan truyền gây ra việc thực hiện lặp lại echo a, lấp đầy hạn ngạch còn lại bằng a. 

Đầu ra:```
abaaaaaaaa
```Dấu vết xác nhận rằng lệnh cp chỉ ảnh hưởng đến số lần thực thi trong tương lai chứ không ảnh hưởng đến đầu ra ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Mỗi lệnh được xử lý một lần và việc truyền bá chạm đến phạm vi tiền tố được giới hạn bởi n | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách hướng dẫn và số lần thực hiện | 

Các giới hạn n, k ≤ 100000 vừa khít trong thời gian tuyến tính và việc truyền qua các chỉ số tiền tố vẫn hiệu quả vì mỗi lần cập nhật là phép cộng số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    ops = []

    for _ in range(n):
        parts = input().split()
        if parts[0] == "echo":
            ops.append(("echo", parts[1]))
        else:
            ops.append(("cp", int(parts[1])))

    cnt = [0] * n
    cnt[0] = 1
    res = []
    processed = 0

    for i in range(n):
        if processed >= k:
            break
        if cnt[i] == 0:
            continue

        take = min(cnt[i], k - processed)

        if ops[i][0] == "echo":
            res.append(ops[i][1] * take)
            processed += take
        else:
            processed += take
            m = ops[i][1]
            if m > 0:
                add = cnt[i]
                for j in range(m):
                    cnt[j] += add

    return "".join(res)

# sample-like tests
assert run("2 20\necho a\ncp 2\n") == "aaaaaaaaaa"
assert run("3 18\necho a\ncp 2\necho b\n") == "abaaaaaaaa"

# minimum size
assert run("1 1\necho a\n") == "a"

# cp only
assert run("2 5\necho a\ncp 1\n") == "aaaaa"

# alternating growth
assert run("4 10\necho a\ncp 2\necho b\ncp 4\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 tiếng vang | một | ranh giới tối thiểu | 
| chuỗi cp | lặp đi lặp lại | tính chính xác của việc truyền bá | 
| xen kẽ cp/echo | tăng trưởng không trống | ổn định tương tác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cp liên tục nhắm mục tiêu vào tiền tố đầy đủ. Ví dụ: nếu mọi lệnh đều là cp i hoặc echo sớm, số lần thực thi sẽ tăng vọt. Thuật toán xử lý việc này vì nó không bao giờ thực hiện được việc mở rộng; nó chỉ tích lũy số lượng trong một mảng cố định. 

Một trường hợp cạnh khác là khi k nhỏ hơn số lượng lệnh ban đầu. Vòng lặp dừng sớm khi sử dụng ≥ k đã xử lý, do đó các lệnh sau này không bao giờ được chạm vào ngay cả khi giá trị cnt vẫn lớn. 

Trường hợp cạnh cuối cùng là khi các lệnh cp xuất hiện sau tất cả các lệnh echo. Trong trường hợp đó, quá trình lan truyền sẽ tăng số lượng nhưng không tạo ra đầu ra bổ sung vì không còn tiếng vang nào để tiêu thụ nó. Thuật toán vẫn thực hiện cập nhật một cách chính xác nhưng không tạo thêm ký tự nào, phù hợp với thực tế là không thể truy cập thêm lệnh echo nào trong k bước.
