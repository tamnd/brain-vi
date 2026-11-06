---
problem: 1278A
contest_id: 1278
problem_index: A
name: "Shuffle Hashing"
contest_name: "Educational Codeforces Round 78 (Rated for Div. 2)"
rating: 1000
tags: ["brute force", "implementation", "strings"]
answer: passed_samples
verified: true
solve_time_s: 178
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d9d8e-a568-83ec-a4e1-c0bcb751518e
---

# CF 1278A - Băm ngẫu nhiên 

**Đánh giá:** 1000 
**Tags:** vũ lực, thực hiện, dây 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d9d8e-a568-83ec-a4e1-c0bcb751518e 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi cơ sở đại diện cho một mật khẩu và một chuỗi khác được xác nhận là “băm” của mật khẩu đó. Hàm băm không phải là hàm băm mật mã theo nghĩa thông thường. Thay vào đó, nó được xây dựng bằng cách lấy tất cả các ký tự của mật khẩu, hoán vị chúng theo bất kỳ thứ tự nào, sau đó nhúng hoán vị đó vào đâu đó bên trong một chuỗi lớn hơn cũng chứa các chữ cái viết thường bổ sung tùy ý trước và sau. 

Câu hỏi đặt ra là liệu chuỗi thứ hai có thể được tạo ra theo cách này hay không. Nói cách khác, chúng tôi đang kiểm tra xem liệu chúng tôi có thể tìm thấy chuỗi con của hàm băm có nhiều bộ ký tự khớp chính xác với nhiều bộ ký tự của mật khẩu hay không. Phần còn lại của hàm băm bên ngoài chuỗi con được chọn này có thể là bất kỳ thứ gì. 

Mỗi trường hợp kiểm thử là độc lập nên chúng tôi lặp lại việc kiểm tra này cho nhiều cặp chuỗi. 

Các ràng buộc rất nhỏ: cả hai chuỗi có độ dài tối đa là 100 và có tối đa 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất cứ điều gì phức tạp hơn tổng cộng vài triệu thao tác đơn giản. Ngay cả cách tiếp cận O(n³) vẫn an toàn, nhưng bất cứ điều gì theo cấp số nhân hoặc liên quan đến hoán vị đều không cần thiết. 

Một cách tiếp cận không chính xác phổ biến là thử tạo tất cả các hoán vị của mật khẩu và tìm kiếm chúng bên trong hàm băm. Điều đó thất bại ngay lập tức vì ngay cả mật khẩu 10 chữ cái cũng có 10! hoán vị, vốn đã quá lớn. 

Một lỗi nhỏ khác là chỉ kiểm tra xem mật khẩu có xuất hiện dưới dạng một chuỗi con của hàm băm hay không. Điều đó không chính xác vì các ký tự mật khẩu sau khi sắp xếp lại phải xuất hiện liền kề nhau, không được rải rác. Việc xáo trộn chỉ ảnh hưởng đến thứ tự bên trong khối đã chọn chứ không ảnh hưởng đến các vị trí trên toàn bộ chuỗi. 

Một ví dụ nhỏ phá vỡ logic ngây thơ: 

đầu vào:```
p = "ab"
h = "axb"
```Đây,`"ab"`không xuất hiện dưới dạng một chuỗi con, nhưng không có chuỗi con liền kề có độ dài 2 inch`h`cái đó có cả hai`'a'`Và`'b'`cùng nhau. Vì vậy, câu trả lời thực sự là “KHÔNG”. 

Điều kiện chính xác phụ thuộc vào số lượng ký tự phù hợp bên trong cửa sổ trượt. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét mọi chuỗi con có thể có của`h`, trích xuất độ dài của nó bằng`len(p)`và kiểm tra xem chuỗi con có cùng tần số ký tự như`p`. Đối với mỗi chuỗi con, chúng tôi tính toán lại số đếm từ đầu, chi phí là O(26) hoặc O(n) cho mỗi lần kiểm tra. Vì có các chuỗi con O(n), điều này dẫn đến việc kiểm tra O(n²) và O(n) hoạt động mỗi lần, tạo ra O(n³) trong trường hợp xấu nhất. 

Điều này hoạt động dễ dàng với n ≤ 100 nhưng vẫn nhiều hơn mức cần thiết. 

Quan sát quan trọng là chúng ta chỉ quan tâm liệu có bất kỳ chuỗi con nào của`h`có chính xác nhiều bộ ký tự giống như`p`. Đó là một vấn đề khớp đảo chữ có độ dài cố định cổ điển. Thay vì tính toán lại tần số cho mỗi chuỗi con, chúng tôi duy trì một cửa sổ trượt có kích thước |p| qua`h`, cập nhật số lượng ký tự theo O(1) mỗi bước. 

Chúng tôi so sánh mảng tần số của cửa sổ với mảng tần số của`p`. Vì kích thước bảng chữ cái là không đổi (26 chữ cái viết thường), so sánh là O(26), thực tế là không đổi. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính trên`h`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi con Brute Force + đếm đầy đủ | O(n³) | O(1) | Đã chấp nhận (nhưng chậm) | 
| So sánh tần số cửa sổ trượt | O(26·n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính mảng tần số của chuỗi mật khẩu`p`. Mảng này lưu trữ bao nhiêu lần mỗi chữ cái`'a'`ĐẾN`'z'`xuất hiện. Đây là mẫu chúng ta phải khớp chính xác trong một số chuỗi con của`h`. 
2. Nếu`h`ngắn hơn`p`, chúng ta ngay lập tức trả về “KHÔNG” vì không có chuỗi con nào của`h`có thể chứa tất cả các ký tự của`p`. 
3. Xây dựng mảng tần số cho cửa sổ đầu tiên của`h`có độ dài bằng`p`. Cửa sổ này đại diện cho một chuỗi con ứng cử viên. 
4. So sánh tần số cửa sổ với tần số mật khẩu. Nếu chúng khớp nhau, chúng tôi trả lời “CÓ” ngay lập tức. 
5. Trượt cửa sổ lần lượt từng ký tự`h`. Ở mỗi bước: 

- Loại bỏ phần đóng góp của ký tự gửi đi (phía bên trái cửa sổ). 
- Thêm ký tự vào (phía bên phải cửa sổ). 

Điều này giữ cho tần số cửa sổ được cập nhật ở O(1). 
6. Sau mỗi lần cập nhật, hãy so sánh tần suất cửa sổ được cập nhật với tần suất mật khẩu. Nếu tại bất kỳ điểm nào chúng khớp nhau, chúng tôi trả về “CÓ”. 
7. Nếu chúng tôi quét xong tất cả các cửa sổ mà không khớp, chúng tôi sẽ trả về “KHÔNG”. 

Lý do thủ tục này đúng là vì mọi phân đoạn liền kề có thể có của`h`chiều dài`|p|`được kiểm tra chính xác một lần với tư cách là một ứng cử viên và mọi ứng cử viên đều được kiểm tra xem có phải là một hoán vị của`p`. 

### Tại sao nó hoạt động 

Bất biến chính là ở mỗi bước của cửa sổ trượt, mảng tần số của cửa sổ phản ánh chính xác nhiều ký tự của chuỗi con hiện tại của`h`chiều dài`|p|`. Vì mọi vị trí có thể có của length-|p| chuỗi con được truy cập một lần, chúng tôi không bao giờ bỏ lỡ một ứng cử viên hợp lệ. Ngược lại, bất kỳ chuỗi con phù hợp nào cũng phải xuất hiện dưới dạng một trong các cửa sổ này, vì vậy nếu không có chuỗi nào khớp thì không có cấu trúc hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_one(p, h):
    n, m = len(p), len(h)
    if m < n:
        return "NO"
    
    def freq(s):
        f = [0] * 26
        for c in s:
            f[ord(c) - 97] += 1
        return f
    
    fp = freq(p)
    window = freq(h[:n])
    
    if window == fp:
        return "YES"
    
    for i in range(n, m):
        window[ord(h[i]) - 97] += 1
        window[ord(h[i - n]) - 97] -= 1
        if window == fp:
            return "YES"
    
    return "NO"

def main():
    t = int(input())
    out = []
    for _ in range(t):
        p = input().strip()
        h = input().strip()
        out.append(solve_one(p, h))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp được cấu trúc xung quanh một trình trợ giúp kiểm tra một trường hợp thử nghiệm duy nhất. Các mảng tần số là các danh sách có kích thước cố định đơn giản có độ dài 26, giúp tránh mọi chi phí từ từ điển. Cập nhật trượt được thực hiện tại chỗ, đảm bảo chuyển tiếp liên tục giữa các cửa sổ. 

Một lỗi triển khai phổ biến là quên xử lý riêng cửa sổ ban đầu. Một lỗi khác là cập nhật sai mảng tần số, đặc biệt là trộn lẫn các chỉ số đầu ra và đầu vào. Vì cả hai thao tác đều là các phép tăng và giảm đối xứng, nên việc hoán đổi chúng một cách âm thầm sẽ phá vỡ tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
p = "ab"
h = "aab"
```Kích thước cửa sổ là 2. 

| Bước | Cửa sổ | tần số(cửa sổ) | khớp với p | 
| --- | --- | --- | --- | 
| ban đầu | "aa" | a=2,b=0 | không | 
| di chuyển | "ab" | a=1,b=1 | vâng | 

Ở cửa sổ thứ hai, tần số khớp chính xác với mật khẩu nên câu trả lời là CÓ. Điều này chứng tỏ chuỗi con chính xác có thể được dịch chuyển bên trong một chuỗi lớn hơn với các ký tự bổ sung xung quanh nó như thế nào. 

### Ví dụ 2 

đầu vào:```
p = "abc"
h = "abac"
```Kích thước cửa sổ là 3. 

| Bước | Cửa sổ | tần số(cửa sổ) | khớp với p | 
| --- | --- | --- | --- | 
| ban đầu | "aba" | a=2,b=1,c=0 | không | 
| di chuyển | "bạc" | a=1,b=1,c=1 | vâng | 

Ở đây hoán vị hợp lệ của`"abc"`xuất hiện ngay cả khi thứ tự ban đầu không được giữ nguyên. Điều này xác nhận rằng thuật toán xử lý chính xác các hoán vị tùy ý của mật khẩu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · | h | 
| Không gian | O(1) | Chỉ sử dụng mảng tần số có kích thước cố định | 

Cho rằng cả hai chuỗi đều có độ dài tối đa là 100 và có tối đa 100 trường hợp thử nghiệm, tổng số thao tác là không đáng kể. Ngay cả trong trường hợp xấu nhất, chúng tôi chỉ thực hiện vài trăm nghìn lần cập nhật liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    output = sio.StringIO()
    with redirect_stdout(output):
        main()
    return output.getvalue().strip()

# provided samples
assert run("""5
abacaba
zyxaabcaabkjh
onetwothree
threetwoone
one
zzonneyy
one
none
twenty
ten
""") == """YES
YES
NO
YES
NO"""

# minimal match
assert run("""1
a
a
""") == "YES"

# simple negative
assert run("""1
ab
cdabx
""") == "NO"

# permutation inside long string
assert run("""1
abc
zzcbaqq
""") == "YES"

# repeated characters edge case
assert run("""1
aab
ababa
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / a`| CÓ | trường hợp bình đẳng tối thiểu | 
|`ab / cdabx`| KHÔNG | không có cửa sổ hoán vị hợp lệ | 
|`abc / zzcbaqq`| CÓ | phát hiện hoán vị nhúng | 
|`aab / ababa`| CÓ | xử lý ký tự lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mật khẩu chứa các ký tự lặp lại. Trong những trường hợp như vậy, các hoán vị không phải là duy nhất và việc kiểm tra chuỗi con đơn giản dựa trên thứ tự sẽ không thành công. Ví dụ: 

đầu vào:```
p = "aab"
h = "ababa"
```Câu trả lời đúng là CÓ vì chuỗi con`"aba"`hoặc`"baa"`cả hai đều phù hợp với tần số yêu cầu. 

Cửa sổ trượt xử lý chính xác việc này vì nó chỉ theo dõi số lượng chứ không theo dõi thứ tự. Mỗi cửa sổ được so sánh hoàn toàn dựa trên sự bằng nhau của nhiều tập hợp, vì vậy các bản sao được hỗ trợ một cách tự nhiên. 

Một trường hợp cạnh khác là khi`h`ngắn hơn`p`. Ví dụ:```
p = "abc"
h = "ab"
```Thuật toán ngay lập tức từ chối điều này trước khi xử lý vì không có cửa sổ hợp lệ nào tồn tại. Điều này tránh được việc tính toán không cần thiết và ngăn chặn logic trượt ngoài giới hạn. 

Trường hợp tinh tế cuối cùng là khi chuỗi con hợp lệ xuất hiện ở cuối`h`. Việc khởi tạo cộng với vòng trượt đầy đủ đảm bảo cửa sổ cuối cùng được kiểm tra rõ ràng, do đó không bỏ sót trường hợp biên nào.
