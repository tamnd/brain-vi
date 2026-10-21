---
title: "CF 105838D - Thằn Lằn Nhát V"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có $n$ hang động được dán nhãn từ 1 đến $n$ và một chuỗi các cuộc kiểm tra theo kế hoạch $n$, trong đó mỗi cuộc kiểm tra nhắm vào một hang động. Trình tự được cố định và biết trước."
date: "2026-06-22T01:20:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105838
codeforces_index: "D"
codeforces_contest_name: "The 14th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 105838
solve_time_s: 65
verified: true
draft: false
---

[CF 105838D - Thằn lằn hèn nhát V](https://codeforces.com/problemset/problem/105838/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có$n$hang động được dán nhãn từ 1 đến$n$, và một dãy$n$các cuộc kiểm tra theo kế hoạch, trong đó mỗi cuộc kiểm tra nhắm vào một hang động. Trình tự được cố định và biết trước. 

Người chơi phải chọn chính xác một hang động$x$để ẩn vào đó và không thể thay đổi nó sau đó. Mục tiêu là chọn một hang động chưa bao giờ được kiểm tra trong toàn bộ chuỗi. Nói cách khác, chúng ta muốn một giá trị$x$giữa 1 và$n$sao cho nó không xuất hiện ở bất cứ đâu trong mảng. 

Đầu ra cho mỗi trường hợp thử nghiệm là bất kỳ số hang động hợp lệ nào nếu nó tồn tại, nếu không, chúng tôi xuất ra -1 khi mọi hang động từ 1 đến$n$xuất hiện ít nhất một lần. 

Các ràng buộc cho phép lên tới 1000 trường hợp thử nghiệm với tổng số$n$trên tất cả các bài kiểm tra lên đến$2 \cdot 10^5$. Điều này có nghĩa là bất kỳ giải pháp nào gần như tuyến tính cho mỗi trường hợp thử nghiệm đều ổn, nhưng mọi giải pháp bậc hai cho mỗi trường hợp thử nghiệm sẽ quá chậm vì nó có thể giảm xuống còn khoảng$10^{10}$hoạt động trong tình trạng phân phối tồi tệ nhất. 

Một trường hợp tinh vi xuất hiện khi mảng là một hoán vị của$1..n$. Trong trường hợp đó, mỗi số xuất hiện đúng một lần, do đó không có lựa chọn hợp lệ và câu trả lời phải là -1. Một trường hợp cạnh khác là$n = 1$. Nếu giá trị duy nhất là 1 thì câu trả lời là -1; nếu bằng cách nào đó nó không phải là 1 (điều này không thể xảy ra do các ràng buộc), thì nó sẽ là 1. 

Một sai lầm ngây thơ là cho rằng câu trả lời luôn là số còn thiếu nhỏ nhất mà không theo dõi sự hiện diện một cách chính xác hoặc quên rằng chúng ta phải xem xét toàn bộ phạm vi$1..n$, không chỉ các giá trị xuất hiện trong chuỗi. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi hang động ứng cử viên$x$từ 1 đến$n$và với mỗi cái, hãy quét toàn bộ chuỗi để kiểm tra xem nó có xuất hiện hay không. Nếu chúng tôi tìm thấy bất kỳ$x$không bao giờ xuất hiện, chúng tôi xuất nó ngay lập tức. 

Điều này hoạt động vì nó xác minh rõ ràng điều kiện trong vấn đề. Tuy nhiên, chi phí cao. Đối với mỗi$n$ứng viên, chúng tôi có thể quét tới$n$các yếu tố, dẫn đến$O(n^2)$mỗi trường hợp thử nghiệm. Với tổng số$2 \cdot 10^5$, điều này trở nên quá chậm. 

Điều quan trọng là chúng ta không cần phải kiểm tra từng ứng viên một cách độc lập. Chúng tôi chỉ quan tâm đến giá trị nào xuất hiện ít nhất một lần. Khi chúng ta biết tập hợp các giá trị hiện tại, bất kỳ số nào từ 1 đến$n$không có trong bộ đó là hợp lệ. Điều này làm giảm vấn đề thành một cấu trúc đánh dấu vượt qua duy nhất. 

Chúng ta có thể duy trì một mảng kích thước boolean$n$, đánh dấu từng giá trị nhìn thấy, sau đó quét một lần để tìm bất kỳ chỉ mục nào chưa được đánh dấu. Điều này đưa giải pháp xuống thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Đánh dấu/mảng Boolean |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng$a$. Chúng tôi muốn ghi lại những giá trị từ 1 đến$n$xuất hiện. 
2. Tạo một mảng boolean`seen`kích thước$n+1$, được khởi tạo thành sai. Điều này sẽ theo dõi xem mỗi số hang động có xuất hiện trong trình tự kiểm tra hay không. 
3. Duyệt mảng$a$. Với mỗi giá trị$a_i$, đánh dấu`seen[a_i] = true`. Bước này nén tất cả thông tin về chuỗi thành dữ liệu hiện diện. 
4. Sau khi xử lý tất cả các phần tử, lặp lại từ 1 đến$n$. Chỉ số đầu tiên$x$như vậy`seen[x]`sai là câu trả lời hợp lệ vì nó không bao giờ xuất hiện trong chuỗi. 
5. Nếu không như vậy$x$tồn tại, xuất ra -1. Điều này xảy ra chính xác khi mọi số từ 1 đến$n$đã được đánh dấu, nghĩa là dãy chứa tất cả các hang động có thể có. 

Tại sao nó hoạt động: thuật toán giảm vấn đề kiểm tra tư cách thành viên trong một tập hợp. Vì yêu cầu duy nhất là không có trong chuỗi nên mọi giá trị không được đánh dấu là hiện tại sẽ tự động hợp lệ. Nếu tất cả các giá trị được đánh dấu, chuỗi sẽ bao trùm toàn bộ miền, không để lại lựa chọn hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))
        
        seen = [False] * (n + 1)
        for x in a:
            seen[x] = True
        
        ans = -1
        for i in range(1, n + 1):
            if not seen[i]:
                ans = i
                break
        
        out.append(str(ans))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo chiến lược đánh dấu. các`seen`mảng được đặt lại cho mỗi trường hợp thử nghiệm, đảm bảo không có rò rỉ giữa các trường hợp. Vòng lặp đầu tiên nén đầu vào thành thông tin hiện diện. Vòng lặp thứ hai thực hiện quét tuyến tính để xác định bất kỳ giá trị còn thiếu nào. 

Một chi tiết tinh tế là chúng tôi dừng lại ở giá trị còn thiếu đầu tiên thay vì thu thập tất cả các giá trị còn thiếu, vì mọi câu trả lời hợp lệ đều được chấp nhận. Điều này giữ cho logic ở mức tối thiểu và tránh những công việc không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
1 2 3
```| Bước | Đã xem cập nhật mảng | Thiếu quét | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Sau khi xử lý | [_, T, T, T] | kiểm tra 1..3 | - | 
| Quét | 1→T, 2→T, 3→T | không tìm thấy | -1 | 

Ở đây mọi giá trị đều xuất hiện nên không có hang động hợp lệ nào tồn tại. 

### Ví dụ 2 

đầu vào:```
1
5
2 2 3 5 5
```| Bước | Đã xem thông tin cập nhật | Thiếu quét | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Sau khi xử lý | [_, F, T, T, F, T] | bắt đầu từ 1 | 1 | 

Giá trị 1 không bao giờ được nhìn thấy nên nó ngay lập tức hợp lệ và được trả về. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | một lần để chấm điểm + một lần quét để tìm câu trả lời | 
| Không gian |$O(n)$| mảng boolean để theo dõi sự hiện diện | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó thuật toán chạy thoải mái trong giới hạn vì nó chỉ thực hiện một vài lần chuyển tuyến tính qua đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            n = int(input())
            a = list(map(int, input().split()))
            seen = [False] * (n + 1)
            for x in a:
                seen[x] = True
            ans = -1
            for i in range(1, n + 1):
                if not seen[i]:
                    ans = i
                    break
            out.append(str(ans))
        print("\n".join(out))

    from contextlib import redirect_stdout
    buf = io.StringIO()
    with redirect_stdout(buf):
        solve()
    return buf.getvalue().strip()

# provided sample
assert run("1\n1\n1\n") == "-1"

# all equal but missing others
assert run("1\n5\n2 2 2 2 2\n") in {"1", "3", "4", "5"}

# permutation case
assert run("1\n4\n1 2 3 4\n") == "-1"

# single missing at end
assert run("1\n3\n1 2 1\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | bất kỳ chỉ mục nào bị thiếu | trùng lặp không thành vấn đề | 
| hoán vị đầy đủ | -1 | không có câu trả lời hợp lệ tồn tại | 
| n nhỏ có sự lặp lại | phát hiện thiếu chính xác | tính đúng đắn của việc đánh dấu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng là một hoán vị hoàn hảo của$1..n$. Ví dụ:```
1
4
1 2 3 4
```Mảng đánh dấu trở nên hoàn toàn đúng đối với tất cả các chỉ số từ 1 đến 4. Quá trình quét không tìm thấy mục nhập sai nào và thuật toán xuất ra chính xác -1. 

Một trường hợp khác là khi chuỗi chỉ chứa một giá trị lặp lại:```
1
5
2 2 2 2 2
```Ở đây chỉ có chỉ số 2 được đánh dấu là đúng. Quá trình quét ngay lập tức tìm thấy 1 là giá trị còn thiếu đầu tiên và trả về giá trị đó. Điều này cho thấy rằng các bản sao không ảnh hưởng đến tính chính xác vì chúng ta chỉ quan tâm đến sự hiện diện chứ không phải tần suất.
