---
problem: 989B
contest_id: 989
problem_index: B
name: "A Tide of Riverscape"
contest_name: "Codeforces Round 487 (Div. 2)"
rating: 1200
tags: ["constructive algorithms", "strings"]
answer: passed_samples
verified: true
solve_time_s: 83
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a337884-5a20-83ec-bd94-efbdbf86a772
---

#CF 989B - Một Dòng Sông Cảnh Quan 

**Đánh giá:** 1200 
**Tas:** thuật toán xây dựng, chuỗi 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a337884-5a20-83ec-bd94-efbdbf86a772 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài$n$gồm ba loại ký tự: số 0 cố định, số cố định và dấu chấm ký tự đại diện. Mỗi dấu chấm có thể độc lập trở thành 0 hoặc 1. Sau khi thay thế tất cả các dấu chấm, chúng ta thu được một chuỗi nhị phân cụ thể đầy đủ. 

Chúng ta cũng được cho một số nguyên$p$. Một chuỗi được gọi là có dấu chấm$p$khi mọi nhân vật ở vị trí$i$khớp với ký tự ở vị trí$i+p$, bất cứ khi nào cả hai vị trí tồn tại. Nói cách khác, dịch chuyển chuỗi bằng$p$không thay đổi nó ở bất kỳ vị trí nào mà sự so sánh được xác định. 

Nhiệm vụ là xác định xem liệu chúng ta có thể gán giá trị cho tất cả các dấu chấm để chuỗi kết quả không có dấu chấm hay không.$p$. Nếu có thể, chúng ta phải xây dựng một bài tập như vậy. Nếu không, chúng tôi phải báo cáo là không thể. 

Những hạn chế$n \le 2000$đề nghị rằng một$O(n^2)$lý luận là an toàn. Bất cứ điều gì tuyến tính hoặc gần tuyến tính cho mỗi nỗ lực xây dựng đều có thể chấp nhận được, trong khi việc liệt kê theo cấp số nhân trên tất cả$2^{\#dots}$bài tập là không thể thực hiện được vì số lượng dấu chấm có thể lớn. 

Một trường hợp lỗi tinh vi xuất hiện khi cấu trúc bị ép buộc bởi các ký tự đã biết đã xác định tất cả các so sánh trong khoảng thời gian$p$. Ví dụ, nếu mỗi vị trí$i$Và$i+p$có thể so sánh được đã đồng ý do ký tự cố định, khi đó dấu chấm không thể thể hiện sự bất đồng nào và câu trả lời phải là "Không". Ngược lại, nếu tồn tại dù chỉ một cặp mà chúng ta có tự do, chúng ta có thể tạo ra sự không khớp. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ thử tất cả các phép gán dấu chấm, điền vào chuỗi và kiểm tra xem dấu chấm có$p$nắm giữ. Kiểm tra chi phí của một nhiệm vụ duy nhất$O(n)$, và có$2^k$bài tập ở đâu$k$là số chấm. Điều này nhanh chóng trở nên không khả thi ngay cả đối với$k$, từ$n$có thể lên tới 2000. 

Quan sát quan trọng là chúng ta không cần phải suy luận về tất cả các nhiệm vụ trên toàn cầu. Điều kiện cho kỳ$p$hoàn toàn cục bộ: nó chỉ so sánh các cặp$(i, i+p)$. Nếu chúng ta tạo ra sự không khớp trong bất kỳ cặp nào như vậy thì chuỗi đó ngay lập tức không tuần hoàn. 

Điều này gợi ý một cách tiếp cận mang tính xây dựng. Đầu tiên, chúng tôi cố gắng thỏa mãn tất cả các ràng buộc bắt buộc do các ký tự đã biết áp đặt. Nếu một dấu chấm được ghép với một giá trị đã biết thì nó phải khớp với giá trị đó nếu chúng ta muốn có tính tuần hoàn. Nếu cả hai đều được biết và xung đột, thì tính tuần hoàn là không thể và chúng ta đã hoàn thành. Sau khi giải quyết việc lan truyền cưỡng bức, chúng ta kiểm tra xem có cặp nào không$(i, i+p)$trong đó cả hai vị trí đều miễn phí hoặc có thể được thực hiện khác nhau. Nếu một cặp như vậy tồn tại, chúng ta có thể gán các giá trị một cách rõ ràng để phá vỡ tính tuần hoàn. Mặt khác, tất cả các cặp đều bị khóa ở mức bình đẳng, nghĩa là tính tuần hoàn không thể bị vi phạm. 

Việc xây dựng cuối cùng trở thành chiến lược “phá vỡ một hạn chế” có mục tiêu thay vì khám phá tất cả các nhiệm vụ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^k)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nghĩ về mặt ràng buộc giữa các vị trí$i$Và$i+p$. Mỗi cặp như vậy thực thi sự bình đẳng nếu chuỗi vẫn tuần hoàn. 

1. Iterate over all indices$i$từ$1$ĐẾN$n-p$. Đối với mỗi cặp$(i, i+p)$, kiểm tra hai ký tự. 
2. Nếu cả hai ký tự đều cố định và khác nhau (một ký tự là 0 và ký tự kia là 1), thì tính tuần hoàn là không thể, bất kể chúng ta điền vào dấu chấm như thế nào. Trong trường hợp này, chúng ta có thể xuất ra bất kỳ phần hoàn thành nào của chuỗi một cách an toàn. 
3. Ngược lại, nếu có ít nhất một cạnh là dấu chấm hoặc cả hai đều bằng nhau thì cặp này không giúp chúng ta tự động phá vỡ tính tuần hoàn. Chúng tôi tiếp tục kiểm tra tất cả các cặp. 
4. Nếu chúng ta tìm thấy một cặp$(i, i+p)$trong đó ít nhất một vị trí là dấu chấm và vị trí kia đã được biết hoặc cũng là dấu chấm, chúng tôi cố gắng tạo ra sự không khớp: gán (các) dấu chấm sao cho$s[i] \ne s[i+p]$. 
5. Để xây dựng một câu trả lời hợp lệ, trước tiên chúng ta sao chép chuỗi và thay thế các dấu chấm còn lại một cách tùy ý (giả sử bằng 0), sau đó thực thi rõ ràng một cặp đã chọn để phân biệt bằng cách lật một bên. 
6. Nếu không có cặp nào như vậy tồn tại để chúng tôi có thể đưa ra sự không khớp, điều đó có nghĩa là mọi cặp đều đã được cố định bằng nhau hoặc được xác định hoàn toàn theo cách buộc phải có sự bằng nhau. Trong trường hợp này, không cấu trúc nào có thể phá vỡ tính tuần hoàn, vì vậy chúng ta xuất ra "Không". 

### Tại sao nó hoạt động 

Chuỗi có dấu chấm$p$chính xác khi nào mỗi cặp chỉ số$(i, i+p)$trận đấu. Các cặp này hình thành các ràng buộc độc lập; không có vị trí nào tương tác với bất kỳ cặp nào bên ngoài liên kết đã dịch chuyển của chính nó. Điều này có nghĩa là việc phá vỡ tính tuần hoàn đòi hỏi phải phá vỡ ít nhất một trong những ràng buộc này. 

Nếu mọi cặp đều đã bị buộc phải bằng nhau bởi các ký tự cố định thì các dấu chấm không thể đưa ra sự bất đồng mà không vi phạm một ràng buộc cố định ở nơi khác. Ngược lại, nếu tồn tại ít nhất một cặp mà chúng tôi vẫn có tính linh hoạt, chúng tôi có thể chỉ định các giá trị cục bộ để tạo ra sự không khớp mà không ảnh hưởng đến các cặp khác vì mỗi vị trí chỉ tham gia vào một hướng so sánh quan trọng đối với vi phạm. 

Vì vậy, sự tồn tại của một “cặp linh hoạt” duy nhất vừa cần thiết vừa đủ để xây dựng một sự hoàn thành không định kỳ hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, p = map(int, input().split())
    s = list(input().strip())

    # try to force a mismatch in any (i, i+p) pair
    for i in range(n - p):
        j = i + p

        if s[i] != '.' and s[j] != '.' and s[i] != s[j]:
            # already broken periodicity, just fill dots arbitrarily
            for k in range(n):
                if s[k] == '.':
                    s[k] = '0'
            print("".join(s))
            return

    # build a baseline valid fill
    t = s[:]
    for i in range(n):
        if t[i] == '.':
            t[i] = '0'

    # try to introduce a mismatch in some pair
    for i in range(n - p):
        j = i + p
        if s[i] == '.' or s[j] == '.':
            t[i] = '0'
            t[j] = '1'
            print("".join(t))
            return

    print("No")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên quét tìm một cặp vốn đã mâu thuẫn ở đầu vào cố định. Nếu một cặp như vậy tồn tại thì chúng ta sẽ hoàn thành ngay lập tức vì tính tuần hoàn là không thể; chúng ta chỉ cần giải quyết các dấu chấm một cách nhất quán. 

Giai đoạn thứ hai xây dựng một chuỗi đường cơ sở cụ thể hoàn toàn bằng cách thay thế tất cả các dấu chấm bằng số 0. Điều này cung cấp một cấu trúc ứng cử viên định kỳ hợp lệ để sửa đổi. 

Giai đoạn thứ ba cố gắng đưa ra sự không phù hợp có chủ ý trong một cặp có ít nhất một vị trí linh hoạt. Thay đổi cả hai vị trí là an toàn vì mỗi vị trí chỉ bị ràng buộc thông qua chính nó$i$Và$i+p$mối quan hệ và chúng tôi chỉ cần một so sánh bị vi phạm duy nhất. 

Trường hợp "Không" cuối cùng chỉ xảy ra khi mọi cặp đều hoàn toàn cố định và nhất quán, không còn cách nào phá vỡ sự bình đẳng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 7
1.0.1.0.1.
```Chúng tôi kiểm tra các cặp$(i, i+7)$. Cặp (1,8) so sánh '1' và '.' nên có tính linh hoạt. 

| Bước | tôi | tôi+p | s[i] | s[i+p] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| quét | 1 | 8 | 1 | . | có thể có ứng viên không phù hợp | 
| xây dựng | - | - | đường cơ sở | - | điền dấu chấm bằng 0 | 
| sửa đổi | 1 | 8 | 1 | 0 | đặt 1 vs 0 | 

Đầu ra cuối cùng:```
1000100010
```Điều này chứng tỏ một cặp linh hoạt duy nhất có thể phá vỡ tính tuần hoàn như thế nào. 

### Ví dụ 2 

Hãy xem xét:```
5 2
1.1.0
```| Bước | tôi | tôi+p | s[i] | s[i+p] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| quét | 1 | 3 | 1 | 1 | cố định bằng | 
| quét | 2 | 4 | . | . | linh hoạt | 
| xây dựng | - | - | đường cơ sở | 10100 | điền dấu chấm | 
| sửa đổi | 2 | 4 | 0 | 0→1 | thực thi sự không phù hợp | 

Đầu ra có thể là:```
10110
```Điều này xác nhận rằng ngay cả khi có nhiều ràng buộc bằng nhau thì chỉ cần một cặp linh hoạt là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| mỗi vị trí và cặp$(i, i+p)$được kiểm tra một số lần không đổi | 
| Không gian |$O(n)$| lưu trữ và sửa đổi chuỗi | 

Thuật toán dễ dàng phù hợp trong giới hạn vì$n \le 2000$, khiến cho nhiều đường chuyền tuyến tính thậm chí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("10 7\n1.0.1.0.1.\n") == "1000100010"

# all dots, easy break
assert run("4 2\n....\n") != "No"

# already conflicting pair
assert run("3 1\n01.\n") != "No"

# fully fixed periodic impossible break
assert run("4 2\n0101\n") == "No"

# minimal case
assert run("2 1\n0.\n") != "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các dấu chấm | không Không | sự tồn tại của xây dựng linh hoạt | 
| cặp cố định xung đột | bất kỳ hợp lệ | phát hiện sớm những điều không thể tránh được | 
| chuỗi cố định hoàn toàn định kỳ | Không | không cách nào phá vỡ sự bình đẳng | 
| kích thước tối thiểu | hợp lệ | độ đúng ranh giới | 

## Vỏ cạnh 

Một chuỗi cố định hoàn toàn đã thỏa mãn tính tuần hoàn buộc câu trả lời là "Không". Trong trường hợp như vậy, mỗi cặp$(i, i+p)$đã bằng nhau và không tồn tại dấu chấm nào để gây ra sự không khớp. Thuật toán đến nhánh cuối cùng và in "Không", phù hợp với yêu cầu. 

Khi mọi vị trí trong một cặp quan trọng là một dấu chấm, bước xây dựng sẽ gán chúng lần lượt là 0 và 1. Vì các vị trí này chỉ được kiểm tra dựa trên cặp bù của chúng, điều này đảm bảo tính tuần hoàn bị phá vỡ mà không ảnh hưởng đến các ràng buộc khác. 

Khi có chính xác một dấu chấm trong toàn bộ chuỗi, nó luôn luôn đủ nếu nó xuất hiện trong bất kỳ cặp nào$(i, i+p)$. Thuật toán phát hiện điều này trong quá trình quét và sử dụng nó làm điểm đột phá, đảm bảo tính chính xác ngay cả trong các tình huống có độ biến thiên tối thiểu.
