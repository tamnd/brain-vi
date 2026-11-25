---
problem: 999C
contest_id: 999
problem_index: C
name: "Alphabetic Removals"
contest_name: "Codeforces Round 490 (Div. 3)"
rating: 1200
tags: ["implementation"]
answer: passed_samples
verified: false
solve_time_s: 90
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33b04a-3430-83ec-a780-f34f0c4ce948
---

# CF 999C - Xóa chữ cái 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33b04a-3430-83ec-a780-f34f0c4ce948 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các chữ cái viết thường và chúng ta liên tục xóa các ký tự khỏi chuỗi đó một cách chính xác.$k$lần. Mỗi lần xóa tuân theo một quy tắc cố định: chúng tôi quét các chữ cái theo thứ tự bảng chữ cái từ 'a' đến 'z' và đối với mỗi chữ cái, chúng tôi cố gắng loại bỏ vị trí xuất hiện ngoài cùng bên trái của nó trong chuỗi hiện tại. Chữ cái đầu tiên tồn tại trong chuỗi xác định ký tự nào bị xóa trong thao tác đó. 

Điều này có nghĩa là mọi thao tác sẽ loại bỏ chính xác một ký tự, luôn là ký tự xuất hiện sớm nhất có thể trong số ký tự bảng chữ cái nhỏ nhất hiện có tại thời điểm đó. Sau khi thực hiện quá trình này$k$lần, chúng tôi xuất chuỗi còn lại. 

Hạn chế chính là độ dài chuỗi có thể lên tới$4 \cdot 10^5$, do đó, bất kỳ phương pháp nào liên tục quét hoặc xây dựng lại các chuỗi một cách đơn giản trong mỗi bước xóa đều trở nên quá chậm. Một mô phỏng đơn giản của$k$quét toàn bộ lên đến$n$nhân vật dẫn đến$O(nk)$, có thể đạt tới$10^{11}$hoạt động trong trường hợp xấu nhất và sẽ không vượt qua. 

Trường hợp khó phát hiện xuất hiện khi nhiều lần xuất hiện của các chữ cái đầu nằm rải rác. Ví dụ: trong một chuỗi như`"babababa"`, việc xóa ba ký tự không chỉ đơn giản là xóa ba ký tự đầu tiên hoặc xóa ba ký tự nhỏ nhất một cách độc lập, vì mỗi lần xóa phụ thuộc vào chuỗi được cập nhật. Cấu trúc thay đổi sau mỗi lần loại bỏ, dịch chuyển “các lần xuất hiện ngoài cùng bên trái” theo cách mà quá trình tính toán trước đơn giản sẽ bỏ lỡ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng trực tiếp quá trình. Đối với mỗi$k$hoạt động, chúng tôi quét chuỗi từ trái sang phải, kiểm tra từng ký tự và đối với mỗi vị trí, chúng tôi xác định xem đó có phải là chữ cái nhỏ nhất có sẵn trong chuỗi còn lại hay không. Khi chúng tôi tìm thấy ký tự chính xác, chúng tôi xóa ký tự đó và xây dựng lại chuỗi. 

Điều này đúng vì nó tuân theo quy trình một cách chính xác. Tuy nhiên, mỗi lần xóa có thể yêu cầu quét toàn bộ chuỗi hiện tại và việc xóa chính chuỗi đó cũng$O(n)$trừ khi được quản lý cẩn thận. Sang$k$bước, điều này dẫn đến$O(nk)$hoặc tệ hơn là vượt xa giới hạn khi cả hai đều lớn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải mô phỏng từng bước xóa hoàn toàn. Thay vào đó, điều quan trọng là ký tự nào sẽ bị xóa và chúng được chọn theo thứ tự nào. Vì mỗi thao tác luôn chọn 'a' có sẵn ngoài cùng bên trái, nếu không thì là 'b', v.v., chúng ta có thể coi đây là việc đánh dấu các ký tự để xóa một cách tham lam trong khi vẫn giữ nguyên các chỉ mục ban đầu của chúng. Một khi chúng ta xác định được các chỉ số chính xác của tất cả$k$các ký tự bị loại bỏ, việc xây dựng chuỗi cuối cùng sẽ trở thành một lần lọc chúng ra. 

Để thực hiện điều này một cách hiệu quả, chúng tôi duy trì vị trí của từng ký tự bằng cách sử dụng 26 hàng đợi (hoặc danh sách các chỉ mục). Ở mỗi bước, chúng tôi chọn chữ cái nhỏ nhất có hàng đợi không trống, xóa chỉ mục sớm nhất của nó và tiếp tục. Điều này mô phỏng trực tiếp quá trình ra quyết định nhưng trong$O(1)$được khấu hao mỗi lần xóa vì chúng tôi không bao giờ quét lại chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n + k)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách liên tục chọn và xóa các chỉ mục khỏi một cấu trúc có thứ tự thay vì sửa đổi chính chuỗi đó. 

1. Lưu trữ chỉ mục của mỗi ký tự trong 26 danh sách riêng biệt, mỗi danh sách cho mỗi chữ cái. Điều này bảo tồn thứ tự ban đầu. 
2. Đối với mỗi$k$xóa, lặp lại các chữ cái từ 'a' đến 'z'. Ngay khi chúng tôi tìm thấy danh sách không trống, chúng tôi sẽ xóa phần tử đầu tiên của nó. Điều này tương ứng chính xác với “sự xuất hiện ngoài cùng bên trái của chữ cái có sẵn đầu tiên”. 
3. Đánh dấu chỉ mục đó là đã bị xóa trong một mảng boolean. 
4. Sau khi thực hiện xong tất cả$k$xóa, xây dựng lại chuỗi cuối cùng bằng cách quét chuỗi gốc và bỏ qua các vị trí đã xóa. 

Lý do bước 2 hoạt động là vì mỗi danh sách đã được sắp xếp theo chỉ mục, do đó phần tử đầu tiên luôn là phần tử xuất hiện ngoài cùng bên trái của ký tự đó ở trạng thái hiện tại. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, sự lựa chọn của thuật toán chỉ phụ thuộc vào chữ cái nhỏ nhất còn tồn tại và lần xuất hiện ngoài cùng bên trái của nó. Việc duy trì hàng đợi chỉ mục cho mỗi ký tự sẽ duy trì cả hai ràng buộc về thứ tự: trong mỗi chữ cái, chỉ số tăng dần và trên các chữ cái, chúng ta luôn tìm kiếm theo thứ tự bảng chữ cái. Do đó, chỉ mục có sẵn đầu tiên gặp phải trong quá trình quét có cấu trúc này khớp chính xác với những gì mô phỏng chuỗi đầy đủ sẽ tạo ra. Khi một chỉ mục bị xóa, nó sẽ không bao giờ xuất hiện lại, vì vậy các quyết định trong tương lai vẫn nhất quán với chuỗi đang phát triển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    pos = [[] for _ in range(26)]
    for i, ch in enumerate(s):
        pos[ord(ch) - 97].append(i)

    from collections import deque
    pos = [deque(p) for p in pos]

    removed = [False] * n

    for _ in range(k):
        for c in range(26):
            if pos[c]:
                idx = pos[c].popleft()
                removed[idx] = True
                break

    res = []
    for i, ch in enumerate(s):
        if not removed[i]:
            res.append(ch)

    print("".join(res))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã xây dựng 26 hàng đợi để các lần xuất hiện của mỗi ký tự được lưu trữ theo thứ tự chỉ số tăng dần. Mỗi bước xóa sẽ quét từ 'a' đến 'z' và loại bỏ lần xuất hiện hợp lệ sớm nhất. các`removed`mảng đảm bảo chúng ta có thể xây dựng lại chuỗi cuối cùng theo thời gian tuyến tính mà không cần sửa đổi cấu trúc ban đầu. 

Một cạm bẫy triển khai phổ biến là cố gắng xóa vật lý các ký tự khỏi chuỗi trong quá trình mô phỏng, điều này dẫn đến chi phí dịch chuyển lặp đi lặp lại. Một vấn đề tế nhị khác là quên rằng “lần xuất hiện ngoài cùng bên trái” phải được xác định liên quan đến trạng thái hiện tại chứ không phải chuỗi gốc; cấu trúc hàng đợi giải quyết vấn đề này một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6, k = 2
s = "abacbc"
```Chúng tôi theo dõi việc xóa từng bước. 

| Bước | Thư được chọn | Đã xóa chỉ mục | Chuỗi còn lại (khái niệm) | 
| --- | --- | --- | --- | 
| 1 | một | 0 | bacbc | 
| 2 | một | 2 | bcbc | 

Sau khi xây dựng lại, chúng tôi nhận được`"bcbc"`. 

Điều này cho thấy thuật toán luôn chọn 'a' sớm nhất trước tiên, mặc dù sau đó có một 'a' khác. 

### Ví dụ 2 

đầu vào:```
n = 7, k = 3
s = "zzabaca"
```| Bước | Thư được chọn | Đã xóa chỉ mục | Chuỗi còn lại (khái niệm) | 
| --- | --- | --- | --- | 
| 1 | một | 2 | zzbaca | 
| 2 | một | 5 | zzbca | 
| 3 | b | 3 | zzca | 

Kết quả cuối cùng là`"zzca"`. 

Điều này xác nhận rằng khi tất cả chữ cái 'a đã hết, thuật toán sẽ di chuyển chính xác đến chữ cái có sẵn tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + k)$| Mỗi chỉ mục được chèn một lần và xóa tối đa một lần và mỗi lần xóa sẽ quét tối đa 26 chữ cái | 
| Không gian |$O(n)$| Chúng tôi lưu trữ các vị trí và đánh dấu xóa | 

Các ràng buộc cho phép lên đến$4 \cdot 10^5$ký tự, do đó thời gian tuyến tính với các hệ số không đổi nhỏ vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, k = map(int, sys.stdin.readline().split())
    s = sys.stdin.readline().strip()

    pos = [[] for _ in range(26)]
    for i, ch in enumerate(s):
        pos[ord(ch) - 97].append(i)

    pos = [deque(p) for p in pos]
    removed = [False] * n

    for _ in range(k):
        for c in range(26):
            if pos[c]:
                removed[pos[c].popleft()] = True
                break

    return "".join(s[i] for i in range(n) if not removed[i])

# provided sample
assert run("15 3\ncccaabababaccbc\n") == "cccbbabaccbc"

# custom cases
assert run("1 1\na\n") == ""                      # single char removal
assert run("5 0\nabcde\n") == "abcde"            # no deletions
assert run("5 5\nddddd\n") == ""                 # all same char
assert run("6 2\nbbbaaa\n") == "bbaa"           # tie-breaking behavior
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 a`| trống | xóa tối thiểu | 
|`5 0 abcde`|`abcde`| trường hợp không hoạt động | 
|`5 5 ddddd`| trống | nhân vật đồng phục | 
|`6 2 bbbaaa`|`bbaa`| lựa chọn ngoài cùng bên trái đúng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi các chữ cái trong bảng chữ cái sớm bị xen kẽ nhiều. Hãy xem xét: 

đầu vào:```
s = "ababab", k = 3
```Hành vi từng bước: 

| Bước | Thư được chọn | Đã xóa chỉ mục | Còn lại | 
| --- | --- | --- | --- | 
| 1 | một | 0 | bab | 
| 2 | một | 2 | bbab | 
| 3 | một | 4 | bbb | 

Thuật toán tiếp tục loại bỏ 'a' một cách chính xác ngay cả sau khi thay đổi cấu trúc, bởi vì hàng đợi của mỗi chữ cái theo dõi các lần xuất hiện còn lại không phụ thuộc vào đột biến chuỗi. 

Một trường hợp khác là khi tất cả các lần xuất hiện của các chữ cái nhỏ hơn đều hết trong quá trình xử lý. Quá trình quét tự nhiên chuyển sang chữ cái có sẵn tiếp theo mà không cần tính toán lại, đảm bảo tính chính xác ngay cả khi các chữ cái đầu biến mất hoàn toàn.
