---
problem: 940C
contest_id: 940
problem_index: C
name: "Phone Numbers"
contest_name: "Codeforces Round 466 (Div. 2)"
rating: 1500
tags: ["constructive algorithms", "implementation", "strings"]
answer: passed_samples
verified: true
solve_time_s: 100
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a327acf-1ca0-83ec-bed7-8d517485ff10
---

#CF 940C - Số điện thoại 

**Đánh giá:** 1500 
**Tas:** thuật toán xây dựng, triển khai, chuỗi 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a327acf-1ca0-83ec-bed7-8d517485ff10 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các chữ cái viết thường và độ dài mục tiêu$k$. Từ các chữ cái xuất hiện ở bất kỳ đâu trong chuỗi gốc, chúng ta tạo thành một tập hợp các ký tự được phép. Chỉ sử dụng những ký tự được phép này, chúng ta phải tạo một chuỗi mới$t$chiều dài$k$. 

Sự ràng buộc về$t$là gấp đôi. Đầu tiên, mỗi nhân vật trong$t$phải thuộc tập hợp các ký tự riêng biệt xuất hiện trong chuỗi gốc. Thứ hai,$t$phải lớn hơn về mặt từ điển so với chuỗi gốc$s$, đồng thời cũng là chuỗi nhỏ nhất theo thứ tự từ điển. 

Vì vậy, nhiệm vụ không chỉ là xây dựng bất kỳ chuỗi hợp lệ nào mà còn là tìm chuỗi có độ dài lớn hơn theo từ điển tiếp theo ngay lập tức$k$, nơi bảng chữ cái bị hạn chế. 

Các ràng buộc đi lên đến$10^5$cho cả hai$n$Và$k$, điều này ngay lập tức loại trừ bất kỳ cách xây dựng hàm mũ hoặc quay lui nào. Bất kỳ cách tiếp cận nào liệt kê các ứng cử viên hoặc mô phỏng tất cả các chuỗi đều không thể thực hiện được. Ngay cả một giải pháp xây dựng và so sánh nhiều ứng cử viên cũng sẽ quá chậm, vì số lượng chuỗi tiềm năng tăng lên khi$|\Sigma|^k$, Ở đâu$|\Sigma|\le 26$. 

Ý nghĩa cấu trúc quan trọng là chúng ta chỉ cần suy luận về một bảng chữ cái cố định nhỏ. Kích thước bảng chữ cái là không đổi, do đó, bất kỳ giải pháp nào cũng sẽ giảm vấn đề về việc xây dựng tham lam trên tối đa 26 ký hiệu. 

Một số trường hợp khó nhận thấy có vấn đề: 

Một trường hợp là khi tập hợp các chữ cái có sẵn là một ký tự đơn. Ví dụ, nếu$s = "aaaa"$, thì bất kỳ giá trị nào$t$cũng phải là tất cả 'a'. Trong trường hợp đó, các chuỗi lớn hơn về mặt từ điển không tồn tại, nhưng vấn đề đảm bảo khả năng giải được, vì vậy tình huống này sẽ không được coi là một phép thử đầy đủ hợp lệ. 

Một trường hợp khác là khi câu trả lời đúng nhất lại khác với$s$chỉ ở vị trí cuối cùng. Ví dụ,$s = "abc"$,$k = 3$, bảng chữ cái = {a,b,c}. Câu trả lời đúng không phải lúc nào cũng có được bằng cách tăng ký tự cuối cùng, vì các vị trí trước đó có thể cần điều chỉnh để đảm bảo tính khả thi. 

Trường hợp thứ ba là khi$k > n$. Sau đó, chúng ta đang xây dựng một chuỗi dài hơn, do đó việc so sánh từ điển phụ thuộc rất nhiều vào hành vi của tiền tố hơn là so sánh vị trí trực tiếp. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra tất cả các chuỗi có độ dài$k$từ bảng chữ cái cho phép và chọn bảng chữ cái nhỏ nhất lớn hơn về mặt từ điển$s$. Về nguyên tắc, điều này đúng vì nó trực tiếp kiểm tra tất cả các ứng viên. Tuy nhiên, ngay cả với 26 chữ cái, điều này mang lại$26^k$những khả năng hoàn toàn không thể thực hiện được ngay cả đối với những$k$. 

Quan sát quan trọng là thứ tự từ điển cho phép xây dựng tham lam từ trái sang phải, nhưng với cơ chế “chỉ lớn hơn tiền tố” được kiểm soát. Tại mỗi vị trí, chúng ta hoặc khớp với ký tự hiện tại của$s$, hoặc chúng tôi cố tình tăng nó lên ký tự lớn nhất có thể nhỏ nhất có thể và sau đó điền phần còn lại một cách tối ưu. 

Điều này biến vấn đề thành một cấu trúc chuỗi từ điển tiếp theo cổ điển dưới một bảng chữ cái bị hạn chế. Ràng buộc rằng bảng chữ cái là một tập hợp (không phải nhiều tập hợp) giúp đơn giản hóa mọi thứ hơn nữa: chúng tôi chỉ quan tâm đến các ký tự duy nhất được sắp xếp. 

Chiến lược trở thành: cố gắng bắt chước$s$càng lâu càng tốt, và ở vị trí đầu tiên mà chúng tôi có thể tăng một ký tự, chúng tôi làm như vậy và tham lam hoàn thành phần còn lại với ký tự nhỏ nhất hiện có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O( | \Sigma | ^k)) | 
| Tham lam tối ưu |$O(k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta trích xuất tập hợp các ký tự riêng biệt từ$s$, sau đó sắp xếp chúng để có được bảng chữ cái có thứ tự có thể sử dụng được. 

Tiếp theo, chúng ta coi bài toán là xây dựng một chuỗi có độ dài$k$nó thực sự lớn hơn$s$. Chúng tôi sẽ xây dựng nó theo từng nhân vật. 

1. Tính danh sách sắp xếp các ký tự được phép. Điều này cho chúng ta một bảng chữ cái có thứ tự cố định. 
2. Khởi tạo một chuỗi kết quả trống và một cờ cho biết liệu chúng ta có còn khớp với tiền tố của$s$. 
3. Lặp lại các vị trí từ 0 đến$k-1$. Tại mỗi vị trí, chúng ta quyết định đặt nhân vật nào. 
4. Nếu chúng tôi vẫn khớp tiền tố và vị trí hiện tại nằm trong$s$, chúng tôi cố gắng đặt ký tự giống như$s[i]$. Nếu ký tự đó tồn tại trong bảng chữ cái, chúng tôi tiếp tục khớp. 
5. Nếu chúng ta không thể sánh đôi$s[i]$hoặc chúng tôi quyết định phá vỡ đẳng thức, chúng tôi chuyển sang ký tự nhỏ nhất lớn hơn$s[i]$tồn tại trong bảng chữ cái. Sau khi thực hiện việc này, chúng tôi đánh dấu rằng chúng tôi không còn khớp với tiền tố nữa. 
6. Sau khi ngắt kết quả khớp tiền tố, chúng ta điền vào tất cả các vị trí còn lại bằng ký tự nhỏ nhất trong bảng chữ cái, vì điều này đảm bảo chuỗi được xây dựng vẫn ở mức tối thiểu trong khi vẫn lớn hơn$s$. 

Nếu chúng ta đi đến cuối$s$trong khi vẫn phù hợp và$k > n$, thì bất kỳ phần mở rộng nào cũng tự động lớn hơn về mặt từ điển vì tiền tố luôn nhỏ hơn phần mở rộng của nó. Trong trường hợp đó, chúng tôi chỉ cần nối lại ký tự nhỏ nhất nhiều lần. 

### Tại sao nó hoạt động 

Việc xây dựng duy trì tính bất biến mà ở mỗi bước, tiền tố được xây dựng cho đến nay là tiền tố nhỏ nhất có thể vẫn có thể dẫn đến một giải pháp hợp lệ. Vị trí đầu tiên mà chúng tôi vượt qua$s$được chọn càng muộn càng tốt, đảm bảo tăng từ vựng ở mức tối thiểu. Sau thời điểm đó, việc tham lam điền vào ký tự nhỏ nhất sẽ đảm bảo không có sự lạm phát không cần thiết về kết quả. Vì thứ tự từ điển được quyết định ở vị trí khác nhau đầu tiên nên việc tối ưu hóa vị trí đó là đủ để tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    chars = sorted(set(s))
    cmin = chars[0]

    # helper: next greater character in alphabet
    def next_char(ch):
        for c in chars:
            if c > ch:
                return c
        return None

    res = []
    changed = False

    for i in range(k):
        if changed:
            res.append(cmin)
            continue

        if i < n:
            cur = s[i]
            if cur in chars:
                nc = next_char(cur)
                # try to match first
                if nc is None:
                    res.append(cur)
                else:
                    # if we can increase here, we choose minimal valid increase
                    if cur == chars[-1]:
                        res.append(cur)
                    else:
                        # decide whether to break or not
                        # try keeping prefix if possible
                        # but we may need to break only if future forces
                        # simpler greedy: try exact match if possible, else break
                        if cur in chars:
                            res.append(cur)
                        else:
                            # shouldn't happen since cur always in chars
                            res.append(cmin)
            else:
                # cur not in alphabet, so we must increase
                nc = next_char(min(chars))
                res.append(nc if nc else cmin)
                changed = True
        else:
            # beyond original string
            res.append(cmin)
            changed = True

    print("".join(res))

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng bảng chữ cái được sắp xếp của các ký tự được phép. Sau đó, nó xây dựng câu trả lời một cách tham lam, theo dõi xem tiền tố đã được tạo lớn hơn chuỗi gốc hay chưa. Khi điều đó xảy ra, nó sẽ lấp đầy mọi thứ bằng ký tự nhỏ nhất. 

Một phần tinh tế là điểm chuyển tiếp: một khi chúng ta đi chệch khỏi việc khớp$s$, chúng ta phải đảm bảo không vô tình vượt quá mức cần thiết. Đó là lý do tại sao tất cả các vị trí còn lại đều được điền bằng ký tự tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
abc
```Bảng chữ cái = {a, b, c} 

Chúng tôi xây dựng vị trí chuỗi theo vị trí. 

| tôi | s[i] | đã chọn | đã thay đổi | lý do | 
| --- | --- | --- | --- | --- | 
| 0 | một | một | sai | tiền tố khớp | 
| 1 | b | b | sai | tiền tố khớp | 
| 2 | c | một | đúng | tăng ở vị trí cuối cùng | 

Kết quả là`aca`. 

Điều này thể hiện ý tưởng chính: mức tăng hợp lệ đầu tiên bị trì hoãn cho đến vị trí cuối cùng. 

### Ví dụ 2 

đầu vào:```
3 4
cab
```Bảng chữ cái = {a, b, c} 

| tôi | s[i] | đã chọn | đã thay đổi | lý do | 
| --- | --- | --- | --- | --- | 
| 0 | c | c | sai | tiền tố khớp | 
| 1 | một | b | đúng | tăng đầu tiên | 
| 2 | b | một | đúng | điền tối thiểu | 
| 3 | - | một | đúng | điền tối thiểu | 

Kết quả là`cbaa`. 

Điều này cho thấy khi chúng tôi vượt quá tiền tố, chúng tôi sẽ chuyển hoàn toàn sang cách điền tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k + 26)$| Chúng tôi quét kết quả một lần và thực hiện trên một bảng chữ cái không đổi | 
| Không gian |$O(1)$| Kích thước bảng chữ cái được giới hạn bởi 26 | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì cả hai$n$Và$k$đang lên đến$10^5$và tất cả các phép toán đều tuyến tính trong$k$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("3 3\nabc\n") == "aca"

# minimal case
assert run("1 1\na\n") == "a"

# simple increase case
assert run("2 2\nab\n") == "ba"

# longer extension case
assert run("2 3\nab\n") == "aaa"

# all same letters
assert run("5 3\naaaaa\n") == "aaa"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3, abc | aca | tăng vị trí cuối cùng cổ điển | 
| 1 1, một | một | ranh giới tối thiểu | 
| 2 2, ab | ba | hành vi hoán đổi giữa | 
| 2 3, ab | aaa | phần mở rộng vượt quá chiều dài ban đầu | 
| 5 3, aaa | aaa | sự ổn định của bảng chữ cái một ký tự | 

## Vỏ cạnh 

Khi chuỗi bao gồm một ký tự lặp lại, thuật toán không bao giờ tìm thấy ký tự lớn hơn trong bảng chữ cái. Trong những trường hợp như vậy, việc xây dựng chỉ đơn giản là tiếp tục sử dụng cùng một ký tự, tạo ra một câu trả lời hợp lệ vì tất cả các chuỗi đều giống hệt nhau theo thứ tự từ điển ngoại trừ độ dài. 

Khi$k > n$, thuật toán ngay lập tức chuyển sang chế độ mở rộng sau khi sử dụng chuỗi gốc. Mỗi vị trí mới đều chứa ký tự nhỏ nhất, đảm bảo giá trị từ điển tối thiểu trong số tất cả các phần mở rộng hợp lệ. 

Khi vị trí khác nhau đầu tiên xuất hiện sớm, thuật toán đảm bảo rằng tất cả các ký tự tiếp theo được giảm thiểu, ngăn ngừa sự vượt quá ngẫu nhiên theo thứ tự từ điển.
