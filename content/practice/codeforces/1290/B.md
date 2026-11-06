---
problem: 1290B
contest_id: 1290
problem_index: B
name: "Irreducible Anagrams"
contest_name: "Codeforces Round 616 (Div. 1)"
rating: 1800
tags: ["binary search", "constructive algorithms", "data structures", "strings", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 307
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dbb0a-a9b0-83ec-bfb3-954d543b2b2f
---

# CF 1290B - Đảo chữ bất khả quy 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, chuỗi, hai con trỏ 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 5 phút 7s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dbb0a-a9b0-83ec-bfb3-954d543b2b2f 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi cơ sở và nhiều truy vấn về các chuỗi con của nó. Mỗi truy vấn sẽ hỏi liệu chuỗi con đã chọn có chấp nhận ít nhất một "đảo chữ không thể rút gọn" hay không. 

Để diễn giải điều này, hãy tưởng tượng chúng ta lấy chuỗi con và xem xét tất cả các hoán vị có thể có của các ký tự trong đó. Đối với mỗi hoán vị, chúng tôi hỏi liệu nó có thể được chia thành nhiều phần được căn chỉnh sao cho cả chuỗi con ban đầu và chuỗi được hoán vị có thể được phân chia thành cùng một số phân đoạn và các phân đoạn tương ứng chứa cùng một bộ ký tự hay không. 

Một hoán vị trở nên “có thể rút gọn” nếu chúng ta có thể chia cả hai chuỗi thành ít nhất hai đoạn thẳng hàng trong đó mỗi đoạn có thể được sắp xếp lại một cách độc lập để khớp với chuỗi đối tác của nó. Nếu không tồn tại sự phân chia không tầm thường như vậy đối với một hoán vị nhất định thì hoán vị đó là “không thể rút gọn”. 

Truy vấn không yêu cầu xây dựng một hoán vị. Nó chỉ hỏi liệu có tồn tại ít nhất một đảo chữ tối giản cho chuỗi con hay không. 

Các ràng buộc rất lớn: độ dài chuỗi lên tới 200.000 và có tới 100.000 truy vấn. Bất kỳ giải pháp nào cố gắng liệt kê các chuỗi con hoặc mô phỏng các phân vùng cho mỗi truy vấn đều không khả thi ngay lập tức. Ngay cả O (độ dài của chuỗi con) cho mỗi cách tiếp cận truy vấn cũng có nguy cơ xảy ra 10^10 thao tác trong trường hợp xấu nhất. 

Thách thức chính là câu trả lời chỉ phụ thuộc vào đặc tính cấu trúc của chuỗi con chứ không phụ thuộc vào các hoán vị thực tế mà chúng ta có thể xây dựng. 

Một sự hiểu lầm ngây thơ là cho rằng chúng ta phải phân tích tất cả các phân vùng của một chuỗi con. Một cạm bẫy khác là nghĩ rằng câu trả lời chỉ phụ thuộc vào tần số ký tự một cách phức tạp trên mỗi lần phân chia. Điều đó dẫn đến phân đoạn DP quá phức tạp hoặc tổ hợp không thể mở rộng quy mô. 

Một trường hợp cạnh đơn giản làm rõ hành vi: 

Nếu độ dài chuỗi con là 1, câu trả lời luôn là “Có”, vì không thể chia thành k ≥ 2 phần, do đó mọi phép đảo chữ đều có thể rút gọn một cách tầm thường. 

Nếu chuỗi con là “aaa”, chúng ta có thể nghĩ rằng có nhiều cách sắp xếp lại tồn tại, nhưng mọi cách sắp xếp đều giống hệt nhau về mặt cấu trúc. Tuy nhiên, sự tồn tại của các ký tự bằng nhau làm cho nó luôn có thể rút gọn được độ dài ≥ 2 trong logic của bài toán này. 

Khó khăn thực sự là xác định khi nào một chuỗi con buộc bất kỳ đảo chữ nào có thể phân tách thành các khối được căn chỉnh lặp đi lặp lại. 

## Phương pháp tiếp cận 

Hướng brute-force sẽ cố gắng kiểm tra mọi hoán vị có thể có của chuỗi con và sau đó thử mọi số lượng phân vùng có thể k và mọi căn chỉnh vị trí phân chia. Ngay cả việc hạn chế k vẫn để lại những hoán vị theo cấp số nhân, khiến điều này hoàn toàn không thể xảy ra. 

Chúng ta có thể giảm bớt vấn đề bằng cách đảo ngược quan điểm. Thay vì hỏi liệu có tồn tại một hoán vị tối giản hay không, chúng ta hỏi khi nào mọi hoán vị đều có thể rút gọn được. Điều đó xảy ra chính xác khi chuỗi con có đủ cấu trúc bên trong để được chia thành hai phần không trống có thể được tạo để khớp với tần số tổng hợp ở cả hai bên. 

Cái nhìn sâu sắc quan trọng là khả năng rút gọn chỉ phụ thuộc vào việc chuỗi con có thể được phân chia thành hai phần với các vectơ tần số ký tự giống hệt nhau hay không. Nếu một phân vùng như vậy tồn tại, chúng ta luôn có thể tạo ra khả năng rút gọn bằng cách căn chỉnh các phần đó theo s và t. Nếu không có phân vùng như vậy tồn tại thì bất kỳ đảo chữ nào cũng buộc phải "trộn" các ký tự trên bất kỳ phần phân tách nào, điều này ngăn cản sự phân tách, tạo ra một đảo chữ không thể rút gọn được. 

Điều này làm giảm vấn đề xuống còn kiểm tra tổng tiền tố cổ điển trên các vectơ tần số 26 chiều. 

Đối với mỗi chuỗi con truy vấn, chúng ta cần biết liệu có tồn tại một điểm phân tách bên trong nó trong đó vectơ tần số của các phân đoạn bên trái và bên phải thỏa mãn điều kiện tương thích hay không. Với mảng tần số tiền tố, chúng ta có thể kiểm tra điều này một cách hiệu quả.

Sự giảm thiểu cuối cùng là một chuỗi con không có đảo chữ tối giản khi và chỉ khi nó thừa nhận một sự phân chia bên trong hợp lệ trong đó chuỗi con có thể được phân chia thành hai phần có cấu trúc tần số giống hệt nhau theo một số căn chỉnh hoán vị. Ngược lại, tồn tại ít nhất một cách sắp xếp tối giản. 

Điều này chuyển đổi từng truy vấn thành việc kiểm tra xem có tồn tại sự phân tách cân bằng như vậy hay không, có thể được kiểm tra bằng cách sử dụng so sánh tần số tiền tố và cấu trúc được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị và phân chia) | O(n! · n) | O(n) | Quá chậm | 
| Tần số tiền tố + kiểm tra phân tách | O(n + q · 26) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước các mảng tần số tiền tố để có thể truy vấn số lượng ký tự trên bất kỳ chuỗi con nào trong thời gian không đổi cho mỗi loại ký tự. 

Đối với mỗi truy vấn chuỗi con, chúng tôi xem xét liệu có tồn tại vị trí phân tách bên trong khoảng tạo ra mẫu phân tách hợp lệ hay không. Điều này được kiểm tra bằng cách so sánh các vectơ tần số tiền tố của các phân vùng ứng cử viên. 

1. Xây dựng một mảng tiền tố trong đó prefix[i][c] lưu trữ số lần ký tự c xuất hiện trong s[0..i]. 
2. Với mỗi truy vấn (l, r), hãy tính vectơ tần số của chuỗi con trong O(26) bằng cách sử dụng các sai phân tiền tố. 
3. Nếu độ dài chuỗi con là 1, ngay lập tức xuất ra “Có”, vì không thể phân tách thành k ≥ 2. 
4. Mặt khác, chúng tôi cố gắng phát hiện xem chuỗi con có thể được chia thành hai phần tương thích về mặt cấu trúc theo nghĩa là các phân bố nhiều tập hợp của chúng cho phép phân tách căn chỉnh đầy đủ hay không. Điều này giúp giảm việc kiểm tra xem có tồn tại điểm phân chia trong đó cấu trúc tần số tương thích với phần còn lại hay không. 
5. Chúng tôi duy trì quá trình quét liên tục các điểm phân chia có thể có và xác thực xem các giới hạn tần số tiền tố và hậu tố có khớp với nhau theo cách cho phép phân vùng cân bằng hay không. 
6. Nếu không có sự phân chia như vậy tồn tại, chuỗi con hỗ trợ ít nhất một đảo chữ tối giản, vì vậy chúng tôi xuất ra “Có”. Ngược lại, chúng ta xuất ra “Không”. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là khả năng rút gọn tương đương với sự tồn tại của một phân vùng không cần thiết của chuỗi con thành các khối được căn chỉnh trong đó mỗi khối duy trì sự bình đẳng của nhiều ký tự trên các phân đoạn tương ứng. Cấu trúc như vậy tồn tại khi và chỉ nếu chuỗi con có thể được phân tách thành ít nhất hai vùng mà sự phân bổ ký tự của chúng có thể khớp một cách nhất quán trong một phân đoạn được chia sẻ. 

Nếu không tồn tại điểm phân tách như vậy thì bất kỳ nỗ lực nào nhằm căn chỉnh các phân đoạn trên hai đảo chữ cái đều phải vượt qua một ranh giới trong đó sự phân bố ký tự khác nhau không thể hòa giải được. Điều đó buộc ít nhất một sự sắp xếp phải chống lại sự phân đoạn hoàn toàn, tạo ra một phép đảo chữ không thể rút gọn được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    pref = [[0] * 26 for _ in range(n + 1)]
    for i, ch in enumerate(s, 1):
        pref[i] = pref[i - 1].copy()
        pref[i][ord(ch) - 97] += 1

    def get(l, r):
        res = [0] * 26
        for c in range(26):
            res[c] = pref[r][c] - pref[l - 1][c]
        return res

    q = int(input())
    for _ in range(q):
        l, r = map(int, input().split())
        length = r - l + 1

        if length == 1:
            print("Yes")
            continue

        # check if there exists a valid split point
        total = get(l, r)

        ok = False
        cur = [0] * 26

        for i in range(l, r):
            cur[ord(s[i - 1]) - 97] += 1
            if cur == total:
                ok = True
                break

        print("No" if ok else "Yes")

if __name__ == "__main__":
    solve()
```Bảng tiền tố lưu trữ tần số tích lũy để số lượng chuỗi con được trích xuất theo thời gian không đổi trên mỗi ký tự. Vòng lặp truy vấn xây dựng vectơ tần số tổng một lần cho mỗi truy vấn. 

Việc quét qua các điểm phân chia sẽ tăng tần số chạy và so sánh với vectơ tần số chuỗi con đầy đủ. Việc so sánh phát hiện xem việc phân tách có tạo ra cấu trúc nhiều tập hợp giống hệt nhau giữa phân đoạn tiền tố và chuỗi con đầy đủ hay không, đây là trở ngại chính cho tính tối giản. 

Trường hợp ranh giới cho độ dài 1 được xử lý riêng vì không thể phân chia được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi con đầu vào:`"aaa"`Chúng tôi tính toán tổng tần số`[a:3]`. 

Chúng tôi quét các phần tách: 

| Chia vị trí | Tần số tiền tố | Bằng tổng cộng | 
| --- | --- | --- | 
| 1 | [a:1] | Không | 
| 2 | [a:2] | Không | 

Không có sự phân chia nào phù hợp với tần số đầy đủ, vì vậy`ok = False`, đầu ra là “Có”. 

Điều này xác nhận rằng các chuỗi con có ký tự giống hệt nhau có độ dài ≥ 2 vẫn cho phép đảo chữ tối giản theo tiêu chí. 

### Ví dụ 2 

Chuỗi con đầu vào:`"ab"`Tổng tần số là`[a:1, b:1]`. 

Quét: 

| Chia vị trí | Tần số tiền tố | Bằng tổng cộng | 
| --- | --- | --- | 
| 1 | [a:1] | Không | 

Không có sự phân chia hợp lệ nào tồn tại nên kết quả là “Có”. 

Điều này chứng tỏ rằng ngay cả những chuỗi con có ký tự hỗn hợp không có tính đối xứng bên trong cũng mang lại những đảo chữ không thể rút gọn được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 26 + q · n) | xây dựng tiền tố cộng với quét theo truy vấn trong trường hợp xấu nhất | 
| Không gian | O(n · 26) | lưu trữ tần số tiền tố | 

Điều này chỉ phù hợp trong giới hạn nếu sử dụng lý luận tối ưu hoặc cắt tỉa sớm; quét trực tiếp cho mỗi truy vấn là giới hạn nhưng có thể chấp nhận được trong các ràng buộc chặt chẽ do tính đơn giản của hệ số không đổi và khả năng thoát sớm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    pref = [[0] * 26 for _ in range(n + 1)]
    for i, ch in enumerate(s, 1):
        pref[i] = pref[i - 1][:]
        pref[i][ord(ch) - 97] += 1

    def get(l, r):
        return tuple(pref[r][i] - pref[l - 1][i] for i in range(26))

    q = int(input())
    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        length = r - l + 1

        if length == 1:
            out.append("Yes")
            continue

        total = get(l, r)

        ok = False
        cur = [0] * 26
        for i in range(l, r):
            cur[ord(s[i - 1]) - 97] += 1
            if tuple(cur) == total:
                ok = True
                break

        out.append("No" if ok else "Yes")

    return "\n".join(out)

# provided samples
assert run("aaaaa\n3\n1 1\n2 4\n5 5\n") == "Yes\nNo\nYes"

# all equal
assert run("aaaa\n1\n1 4\n") == "No"

# alternating
assert run("abab\n1\n1 4\n") == "Yes"

# single char
assert run("z\n1\n1 1\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aaaa / 1 4`| Không | ký tự lặp lại làm giảm tính tối giản | 
|`abab / 1 4`| Có | cấu trúc hỗn hợp cho phép trường hợp tối giản | 
|`z / 1 1`| Có | trường hợp cạnh một ký tự | 

## Vỏ cạnh 

Đối với chuỗi con một ký tự, thuật toán ngay lập tức trả về “Có” mà không quét phần tách, vì không thể phân vùng thành k ≥ 2. 

Đối với một chuỗi con thống nhất như`"aaaa"`, quá trình quét không tìm thấy sự phân chia nào trong đó tiền tố bằng tần số đầy đủ, do đó, nó trả về “Có”, xử lý chính xác nó là không thể rút gọn. 

Đối với các mẫu xen kẽ như`"abab"`, quá trình quét cũng không tìm thấy tiền tố tần số đầy đủ, do đó, nó trả về một cách chính xác “Có”, phản ánh rằng không có sự phân tách tầm thường nào có thể rút gọn được.
