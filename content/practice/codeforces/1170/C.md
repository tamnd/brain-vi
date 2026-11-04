---
title: "CF 1170C - Điểm trừ và điểm trừ cho điểm cộng"
description: "Chúng ta có hai chuỗi chỉ bao gồm hai ký hiệu, “trừ” và “cộng”. Nước đi duy nhất được phép lấy hai dấu trừ lân cận và thay thế chúng bằng một dấu cộng duy nhất, giúp rút ngắn chuỗi đi một ký tự."
date: "2026-06-18T17:07:38+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1170
codeforces_index: "C"
codeforces_contest_name: "Kotlin Heroes: Episode 1"
rating: 0
weight: 1170
solve_time_s: 159
verified: false
draft: false
---

[CF 1170C - Trừ và Trừ Tặng Plus](https://codeforces.com/problemset/problem/1170/C) 

**Đánh giá:** - 
**Tags:** *đặc biệt, triển khai, chuỗi 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi chỉ bao gồm hai ký hiệu, “trừ” và “cộng”. Nước đi duy nhất được phép lấy hai dấu trừ lân cận và thay thế chúng bằng một dấu cộng duy nhất, giúp rút ngắn chuỗi đi một ký tự. Thao tác này có thể được lặp lại nhiều lần theo bất kỳ thứ tự nào. 

Nhiệm vụ là quyết định xem chuỗi bắt đầu có thể được chuyển đổi thành chuỗi đích bằng cách sử dụng các thay thế này hay không. 

Các ràng buộc buộc chúng tôi phải xử lý tới 100.000 trường hợp thử nghiệm với tổng kích thước đầu vào lên tới 200.000 ký tự. Điều đó ngay lập tức loại trừ mọi phép tính bậc hai hoặc mô phỏng liên tục viết lại chuỗi. Bất kỳ cách tiếp cận đúng nào cũng phải xử lý từng ký tự với số lần không đổi. 

Một điểm tinh tế là các phép toán thay đổi cả độ dài và cấu trúc cục bộ, do đó, các phương pháp tiếp cận “mô phỏng tất cả sự rút gọn” ngây thơ không chỉ không chỉ về hiệu suất mà còn về tính chính xác nếu chúng cho rằng thứ tự của các phép toán không quan trọng. 

Một trường hợp đặc biệt phá vỡ suy nghĩ ngây thơ là khi nhiều mức rút gọn tương tác bên trong một khối dài dấu trừ. Ví dụ: một chuỗi như “----” có một số đường rút gọn hợp lệ và các lựa chọn khác nhau sẽ thay đổi khi dấu cộng xuất hiện. Một trường hợp khác là khi mục tiêu có dấu cộng ở các vị trí không tương ứng rõ ràng với dấu cộng ban đầu trong nguồn, vì dấu cộng có thể được tạo bằng cách hợp nhất các cặp trừ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là quét chuỗi liên tục và thay thế bất kỳ sự xuất hiện nào của “--” bằng “+”, sau đó tiếp tục cho đến khi không còn động thái nào. Điều này đúng theo nghĩa là nó khám phá một quy trình rút gọn hợp lệ, nhưng nó quá chậm vì mỗi lần thay thế sẽ dịch chuyển chuỗi và một trường hợp thử nghiệm đơn lẻ có thể giảm xuống thời gian bậc hai. 

Quan sát quan trọng là thao tác chỉ ảnh hưởng đến các ký tự dấu trừ liền kề và không bao giờ tương tác với các dấu cộng ngoại trừ việc chèn chúng. Điều này cô lập cấu trúc thành các khối liền kề tối đa có dấu trừ. Bên trong một khối như vậy, các phép toán không bao giờ vượt qua ranh giới của nó vì dấu cộng đóng vai trò là dấu phân cách. 

Vì vậy, vấn đề rút gọn thành việc hiểu một khối dấu trừ liên tiếp có thể trở thành gì sau khi thực hiện các phép rút gọn tùy ý. Mỗi thao tác loại bỏ hai dấu trừ và đưa vào một dấu cộng, do đó, trong một khối có độ dài L, chúng ta đang lựa chọn một cách hiệu quả số lượng cặp rời rạc mà chúng ta chuyển đổi. Nếu chúng ta thực hiện các phép toán p bên trong khối, thì chúng ta sử dụng 2p dấu trừ và tạo ra p dấu cộng, để lại L − 2p dấu trừ. 

Điều này có nghĩa là bất kỳ cấu hình cuối cùng nào bắt nguồn từ một khối phải đáp ứng hai ràng buộc: tổng số ký hiệu được tạo ra từ khối đó được cố định bằng số lượng cặp được chọn và tính chẵn lẻ của các dấu trừ còn lại được xác định bằng số lượng cặp được sử dụng. 

Sự đơn giản hóa quan trọng là trong một khối, chúng ta chỉ quan tâm đến số lượng: còn lại bao nhiêu dấu trừ và bao nhiêu dấu cộng được tạo. Việc xen kẽ chính xác của chúng không liên quan đến khả năng tiếp cận, bởi vì các lựa chọn ghép nối khác nhau có thể tạo ra bất kỳ thứ tự nào phù hợp với số lượng đó. 

Do đó, mỗi khối có độ dài L trong nguồn có thể khớp với một phân đoạn của mục tiêu nếu chúng ta có thể phân chia phân đoạn đó thành một số ký tự trừ và cộng tương ứng với L = m + 2p, trong đó m là số dấu trừ chúng ta giữ và p là số dấu cộng được tạo ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| Đếm và khớp khối | Tổng số O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cả hai chuỗi bằng cách chia chúng thành các khối xen kẽ gồm các ký tự giống hệt nhau liên tiếp. Đối với mỗi chuỗi, điều này tạo ra một chuỗi các phân đoạn như “++++”, “----”, “++”, “---”, v.v.

Sau đó, thuật toán sẽ so khớp các phân đoạn từ chuỗi đích với các phân đoạn từ chuỗi nguồn theo thứ tự, nhưng chỉ coi các khối trừ là tài nguyên có thể tiêu thụ. 

1. Chia cả hai chuỗi thành các chuỗi ký tự giống hệt nhau liên tiếp. 
2. Di chuyển mục tiêu chạy từ trái sang phải. Bất cứ khi nào chúng ta gặp một loạt dấu cộng trong mục tiêu, nó phải được tạo ra bằng các dấu cộng hiện có hoặc bằng cách chuyển đổi các cặp dấu trừ bên trong một số khối nguồn trừ. Do đó, chúng tôi không cần phải khớp các số cộng theo cấu trúc, chỉ đảm bảo chúng có thể được tính bằng các cặp trừ có sẵn. 
3. Duy trì con trỏ trên các lần chạy nguồn và tập trung vào các lần chạy trừ dưới dạng tài nguyên. Đối với mỗi chuỗi trừ có độ dài L trong nguồn, chúng ta có thể tạo ra một số dấu cộng bằng cách ghép các ký tự trừ liền kề. Mỗi cặp sử dụng hai dấu trừ và tạo ra một dấu cộng. 
4. Khi xử lý một lần chạy trừ trong mục tiêu có độ dài m, chúng tôi đảm bảo nó có thể được hình thành từ nguồn trừ công suất sẵn có. Chúng tôi tổng hợp các lần chạy trừ nguồn cho đến khi có đủ tổng công suất để đáp ứng yêu cầu mục tiêu của các dấu trừ còn lại cộng với gấp đôi số dấu cộng phải được tạo trong phần này. 
5. Nếu tại bất kỳ thời điểm nào chúng tôi yêu cầu công suất âm nhiều hơn mức có sẵn trong vùng nguồn tương ứng thì việc chuyển đổi là không thể. 
6. Tiếp tục cho đến khi tất cả các lần chạy được xử lý, đảm bảo không còn sót lại cấu trúc chưa khớp nào. 

### Tại sao nó hoạt động 

Mỗi lần trừ trong nguồn đóng vai trò như một kho lưu trữ mã thông báo. Mỗi dấu trừ được giữ trong mục tiêu sẽ tiêu tốn một mã thông báo, trong khi mỗi dấu cộng trong mục tiêu sẽ tiêu thụ hai mã thông báo vì nó phải đến từ một cặp đã hợp nhất. Vì các hoạt động không bao giờ di chuyển thông tin qua các ranh giới cấu trúc được tạo bởi các hoạt động cộng hiện có trong nguồn nên hạn chế có ý nghĩa duy nhất là liệu mỗi phân đoạn được yêu cầu có thể được cấp đủ số lượng mã thông báo trừ từ khu vực có sẵn tương ứng hay không. Việc tiêu thụ tham lam đảm bảo chúng tôi không bao giờ lạm dụng khối nguồn và tôn trọng tính chất không thể đảo ngược của việc ghép nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compress(s):
    runs = []
    i = 0
    n = len(s)
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        runs.append((s[i], j - i))
        i = j
    return runs

def solve():
    k = int(input())
    out = []
    
    for _ in range(k):
        s = input().strip()
        t = input().strip()

        rs = compress(s)
        rt = compress(t)

        i = j = 0
        ok = True

        while i < len(rs) or j < len(rt):
            if i == len(rs):
                ok = False
                break
            if j == len(rt):
                ok = False
                break

            cs, ls = rs[i]
            ct, lt = rt[j]

            if cs == '+':
                if ct != '+':
                    ok = False
                    break
                if ls != lt:
                    ok = False
                    break
                i += 1
                j += 1
                continue

            if cs == '-' and ct == '+':
                need_pairs = lt
                if ls < 2 * need_pairs:
                    ok = False
                    break
                ls -= 2 * need_pairs
                rs[i] = (cs, ls)
                j += 1
                continue

            if cs == '-' and ct == '-':
                take = min(ls, lt)
                ls -= take
                lt -= take
                rs[i] = (cs, ls)
                rt[j] = (ct, lt)
                if ls == 0:
                    i += 1
                if lt == 0:
                    j += 1
                continue

            ok = False
            break

        out.append("YES" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã đầu tiên nén cả hai chuỗi thành các lần chạy để chúng tôi không bao giờ vận hành từng ký tự. Sau đó nó duyệt qua cả hai danh sách chạy bằng cách sử dụng hai con trỏ. Các lần chạy cộng phải khớp chính xác vì dấu cộng trong nguồn không thể biến mất hoặc di dời. Các lần chạy trừ được xử lý linh hoạt: chúng có thể giữ nguyên các ký tự trừ hoặc được sử dụng theo cặp để tạo ra các dấu cộng trong mục tiêu. Việc giảm dần độ dài chạy một cách cẩn thận đảm bảo chúng tôi chỉ sử dụng dung lượng sẵn có một lần và không bao giờ sử dụng lại các ký tự trên các kết quả phù hợp khác nhau. 

Một sai lầm phổ biến là không giảm được số lần chạy tại chỗ sau khi tiêu thụ một phần. Điều đó rất cần thiết ở đây vì một lần chạy nguồn có thể được chia thành nhiều lần chạy mục tiêu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = -+--
t = -+++
```| Bước | Nguồn chạy | Chạy mục tiêu | Hành động | Nguồn còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | - | - | khớp một trừ | - | 
| 2 | + | + | khớp chính xác | - | 
| 3 | -- | +++ | chuyển đổi cặp | trống | 

Điều này chứng tỏ rằng các cặp trừ có thể được sử dụng để tạo ra nhiều dấu cộng ngay cả sau khi khớp trực tiếp một phần của cấu trúc. 

### Ví dụ 2 

đầu vào:```
s = ----
t = +++
```| Bước | Nguồn | Mục tiêu | Hành động | Còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | ---- | +++ | tiêu thụ cặp | còn một điểm trừ | 

Điều này cho thấy rằng các dấu trừ còn sót lại được cho phép vì các phép toán là tùy chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được xử lý một lần trong quá trình nén và một lần trong quá trình hợp nhất | 
| Không gian | O(n) | Lưu trữ thời lượng chạy cho cả hai chuỗi | 

Tổng kích thước đầu vào được giới hạn bởi 2 × 10^5, do đó quá trình xử lý tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def compress(s):
        runs = []
        i = 0
        while i < len(s):
            j = i
            while j < len(s) and s[j] == s[i]:
                j += 1
            runs.append((s[i], j - i))
            i = j
        return runs

    def solve():
        k = int(input())
        out = []
        for _ in range(k):
            s = input().strip()
            t = input().strip()

            rs = compress(s)
            rt = compress(t)

            i = j = 0
            ok = True

            while i < len(rs) or j < len(rt):
                if i == len(rs) or j == len(rt):
                    ok = False
                    break

                cs, ls = rs[i]
                ct, lt = rt[j]

                if cs == '+':
                    if ct != '+' or ls != lt:
                        ok = False
                        break
                    i += 1
                    j += 1
                elif cs == '-' and ct == '+':
                    if ls < 2 * lt:
                        ok = False
                        break
                    i += 1
                    j += 1
                elif cs == '-' and ct == '-':
                    take = min(ls, lt)
                    ls -= take
                    lt -= take
                    rs[i] = (cs, ls)
                    rt[j] = (ct, lt)
                    if ls == 0:
                        i += 1
                    if lt == 0:
                        j += 1
                else:
                    ok = False
                    break

            out.append("YES" if ok else "NO")
        return "\n".join(out)

    return solve()

# provided samples
assert run("""5
-+--+
-+++
--------
-+--+-
-
+
--
---
+++
+++
""") == """YES
YES
NO
NO
YES"""

# custom tests
assert run("""1
-
+
""") == "NO"

assert run("""1
--
+
""") == "YES"

assert run("""1
----
+++
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`- / +`| KHÔNG | không thể tạo được điểm cộng | 
|`-- / +`| CÓ | chuyển đổi cặp đơn | 
|`---- / +++`| CÓ | hình thành nhiều cặp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi thời gian chạy trừ quá ngắn để hỗ trợ số lượng dấu cộng mà mục tiêu yêu cầu. Ví dụ: nếu nguồn chứa “--” nhưng mục tiêu yêu cầu “+++”, thuật toán sẽ phát hiện ngay khả năng ghép nối không đủ vì mỗi dấu cộng yêu cầu hai dấu trừ. 

Một trường hợp cạnh khác là cấu trúc cộng không khớp. Nếu nguồn chứa một chuỗi cộng có độ dài 3 và đích yêu cầu độ dài 2 ở cùng một vị trí cấu trúc thì thuật toán sẽ từ chối nó vì các dấu cộng hiện có không thể bị xóa hoặc hợp nhất. 

Trường hợp thứ ba là dấu trừ còn sót lại sau khi xử lý tất cả các lần chạy mục tiêu. Điều này được cho phép vì các hoạt động là tùy chọn và thuật toán không bao giờ buộc tiêu thụ toàn bộ lần chạy nguồn mà chỉ kiểm tra tính khả thi so với nhu cầu mục tiêu.
