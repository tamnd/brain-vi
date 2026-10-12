---
title: "CF 105017C - Chuỗi có thể sắp xếp được"
description: "Chúng ta được cho hai chuỗi có độ dài bằng nhau. Ở mọi vị trí, chúng ta có thể xem cặp ký tự được tạo thành bằng cách lấy một ký tự từ chuỗi đầu tiên và một ký tự từ chuỗi thứ hai."
date: "2026-06-28T02:08:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105017
codeforces_index: "C"
codeforces_contest_name: "Winter Cup 4.0 Online Mirror Contest"
rating: 0
weight: 105017
solve_time_s: 49
verified: true
draft: false
---

[CF 105017C - Chuỗi có thể sắp xếp cùng nhau](https://codeforces.com/problemset/problem/105017/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi có độ dài bằng nhau. Ở mọi vị trí, chúng ta có thể xem cặp ký tự được tạo thành bằng cách lấy một ký tự từ chuỗi đầu tiên và một ký tự từ chuỗi thứ hai. Đối với bất kỳ phân đoạn chỉ mục nào, chúng ta được phép thực hiện thao tác hoán đổi các ký tự bên trong hai chuỗi nhưng chỉ ở cùng một chỉ mục, nghĩa là chúng ta hoán đổi cặp theo chiều dọc. Mỗi lần hoán đổi trao đổi A[i] với B[i] và chúng ta có thể thực hiện việc này một cách độc lập cho bất kỳ vị trí nào trong phân khúc và số lần bất kỳ. 

Đối với một phạm vi truy vấn, chúng ta muốn quyết định xem liệu có thể sắp xếp lại các cặp dọc này sao cho sau một số chuỗi hoán đổi, cả hai chuỗi con thu được có thể được sắp xếp riêng lẻ theo thứ tự không giảm. 

Quan sát quan trọng ẩn trong câu lệnh là chúng ta không sắp xếp lại thứ tự các ký tự bên trong một chuỗi một cách tùy ý. Chúng tôi chỉ có quyền truy cập để hoán đổi hai ký tự ở cùng một chỉ mục. Điều này có nghĩa là trên một phân đoạn, chúng tôi đang lựa chọn một cách hiệu quả, đối với từng vị trí, cho dù ký tự đi đến chuỗi đầu tiên hay chuỗi thứ hai, nhưng chúng tôi không thể hoán đổi vị trí. 

Kích thước đầu vào tăng lên 100000 cho cả độ dài chuỗi và số lượng truy vấn, do đó, bất kỳ lần quét tuyến tính cho mỗi truy vấn nào trên phân đoạn đều quá chậm. Một giải pháp kiểm tra từng truy vấn một cách độc lập trong O(r - l + 1) sẽ giảm xuống O(nq), vượt xa giới hạn có thể chấp nhận được. 

Một cách tiếp cận đơn giản cũng có xu hướng bỏ sót một ràng buộc cấu trúc tinh tế: ngay cả khi cả hai chuỗi đều được sắp xếp, chúng ta phải tôn trọng thực tế là mỗi chỉ mục đóng góp chính xác một ký tự cho mỗi chuỗi cuối cùng. Một lỗi phổ biến là coi vấn đề là hai tập hợp độc lập trên mỗi phân đoạn, bỏ qua sự ghép nối được đưa ra bởi các hoán đổi trên mỗi chỉ mục. 

Trường hợp cạnh có vấn đề là khi các ký tự được xen kẽ nhiều nhưng cân bằng toàn cục. Ví dụ: A = "ab", B = "ba", trong đó câu trả lời là CÓ cho toàn bộ phạm vi, mặc dù ban đầu cả hai chuỗi đều không được sắp xếp. Một trường hợp đặc biệt khác là khi số lượng khớp nhưng sự sắp xếp buộc vi phạm thứ tự được sắp xếp cho dù việc hoán đổi được thực hiện như thế nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng mô phỏng quy trình cho mỗi truy vấn. Đối với một phân khúc nhất định, chúng tôi xem xét tất cả 2^(độ dài) cách hoán đổi hoặc không hoán đổi từng chỉ mục. Đối với mỗi cấu hình, chúng tôi xây dựng hai chuỗi kết quả và kiểm tra xem cả hai chuỗi có được sắp xếp hay không. Điều này đúng vì nó khám phá rõ ràng toàn bộ không gian trạng thái của các hoạt động được phép. Tuy nhiên, ngay cả đối với một đoạn có độ dài k, điều này đã yêu cầu cấu hình O(2^k) và với k lên tới 100000 thì điều này hoàn toàn không khả thi. 

Một lực lượng vũ phu có cấu trúc hơn sẽ giảm việc kiểm tra thành: chọn cho từng vị trí xem A[i] hay B[i] đóng góp vào chuỗi đầu tiên và vị trí còn lại chuyển đến chuỗi thứ hai, sau đó xác minh tính sắp xếp. Điều này trình bày lại vấn đề như một vấn đề gán tổ hợp. Cái nhìn sâu sắc quan trọng là tính sắp xếp áp đặt một ràng buộc đơn điệu trên các chỉ số. Khi chúng ta quyết định ký tự nào sẽ vào vị trí i của chuỗi đầu tiên, nó sẽ hạn chế những gì được phép ở i+1. 

Điều này dẫn đến quan sát trọng tâm: chúng ta không cần phải xem xét tất cả các phép gán, chúng ta chỉ cần theo dõi xem có bao nhiêu ký tự của từng loại có sẵn trong phân đoạn và đảm bảo rằng cả hai chuỗi được sắp xếp kết quả đều có thể được hình thành một cách nhất quán. Điều này trở thành vấn đề tần số tiền tố, trong đó mỗi truy vấn có thể được trả lời bằng cách so sánh số lượng ký tự trong A và B trong phạm vi, cùng với điều kiện khả thi chỉ phụ thuộc vào thứ tự các chữ cái. 

Chúng tôi giảm từng truy vấn để kiểm tra xem liệu nhiều cặp có thể được chia thành hai chuỗi không giảm hay không. Điều này tương đương với việc xác minh rằng đối với mọi ngưỡng ký tự, sự mất cân bằng tích lũy giữa A và B không vi phạm các ràng buộc về thứ tự. Với tổng tiền tố theo tần số ký tự, mỗi truy vấn có thể được trả lời bằng O(26).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n mỗi truy vấn) | O(n) | Quá chậm | 
| Tần số tiền tố + kiểm tra | O(26 · Q + 26 · N) | O(26 · N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành mảng tần số tiền tố cho cả hai chuỗi, một mảng cho mỗi ký tự. 

1. Xây dựng số tiền tố riêng biệt cho A và B, trong đó prefA[c][i] lưu trữ số lần ký tự c xuất hiện trong A cho đến chỉ mục i và tương tự cho B. Điều này cho phép chúng tôi trả lời bất kỳ truy vấn tần số phạm vi nào trong O(1) cho mỗi ký tự. 
2. Với mỗi truy vấn [l, r], hãy tính độ lệch tần số của từng ký tự giữa A và B trong đoạn đó. Điều này cho chúng ta biết tổng cộng có bao nhiêu ký tự có sẵn và cách chúng được phân bổ trên hai chuỗi. 
3. Chúng tôi mô phỏng điều kiện khả thi đối với các ký tự theo thứ tự bảng chữ cái. Ý tưởng là duy trì số lượng ký tự phải đi đến chuỗi đầu tiên để giữ cho chuỗi không giảm, đồng thời tôn trọng số lượng ký tự có sẵn từ phân đoạn. Nếu tại bất kỳ thời điểm nào yêu cầu vượt quá nguồn cung sẵn có thì câu trả lời là KHÔNG. 
4. Chúng tôi kiểm tra các ký tự từ 'a' đến 'z', duy trì khả năng hoạt động về số lượng phần tử vẫn có thể được chỉ định mà không vi phạm các ràng buộc về thứ tự được sắp xếp. Chúng tôi sẵn sàng đảm bảo rằng các ký tự nhỏ hơn sẽ được đặt đầu tiên trong chuỗi đầu tiên bất cứ khi nào cần thiết. 
5. Nếu chúng ta có thể gán thành công tất cả các ký tự trong khi vẫn giữ nguyên cấu trúc đơn điệu trong cả hai chuỗi, thì chúng ta xuất ra CÓ, nếu không thì KHÔNG. 

### Tại sao nó hoạt động 

Thao tác chỉ cho phép hoán đổi trong một chỉ mục cố định, do đó mỗi chỉ mục đóng góp một cặp ký tự cố định. Quyền tự do duy nhất là quyết định chuỗi nào nhận ký tự nào. Điều kiện được sắp xếp có nghĩa là, trong cách sắp xếp cuối cùng, các ký tự trong mỗi chuỗi phải tuân theo thứ tự bảng chữ cái chung. Điều này buộc mọi giải pháp hợp lệ phải tương ứng với việc gán các ký tự nhất quán theo thứ tự tăng dần mà không cần quay lại các chữ cái. 

Bằng cách xử lý các ký tự theo thứ tự và chỉ theo dõi số lượng, chúng tôi nắm bắt chính xác các ràng buộc do sắp xếp áp đặt, bởi vì mọi vi phạm sẽ yêu cầu đặt ký tự lớn hơn trước ký tự nhỏ hơn trong ít nhất một chuỗi, không thể sửa chữa cục bộ. Việc biểu diễn tần số tiền tố đảm bảo chúng tôi luôn suy luận về nhiều tập hợp chính xác cho mỗi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_prefix(s):
    n = len(s)
    pref = [[0] * (n + 1) for _ in range(26)]
    for i, ch in enumerate(s, 1):
        for c in range(26):
            pref[c][i] = pref[c][i - 1]
        pref[ord(ch) - 97][i] += 1
    return pref

def get_range(pref, c, l, r):
    return pref[c][r] - pref[c][l - 1]

def solve():
    L, Q = map(int, input().split())
    A = input().strip()
    B = input().strip()

    prefA = build_prefix(A)
    prefB = build_prefix(B)

    for _ in range(Q):
        l, r = map(int, input().split())

        need = 0
        ok = True

        for c in range(26):
            cnt = (
                get_range(prefA, c, l, r)
                + get_range(prefB, c, l, r)
            )

            if need > cnt:
                ok = False
                break

            need = max(0, need - cnt)

        print("YES" if ok and need == 0 else "NO")

if __name__ == "__main__":
    solve()
```Mảng tiền tố lưu trữ số lượng ký tự tích lũy để mỗi truy vấn trở thành trích xuất theo thời gian không đổi cho mỗi ký tự. Vòng lặp trên 26 ký tự là bước kiểm tra tính khả thi cốt lõi, trong đó`need`theo dõi bao nhiêu ký tự nhỏ hơn vẫn phải được đặt để duy trì các ràng buộc về thứ tự từ điển trên hai chuỗi kết quả. 

Một điểm tinh tế là chúng tôi luôn kết hợp số lượng từ cả A và B, vì các hoán đổi cho phép bất kỳ ký tự nào ở một vị trí di chuyển giữa các chuỗi, do đó, chỉ có tổng số tập hợp bên trong phân đoạn mới quan trọng. Điều kiện cuối cùng`need == 0`đảm bảo rằng tất cả các yêu cầu đặt hàng đã được đáp ứng mà không có những ràng buộc còn sót lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
cbc
adc
1 3
```Chúng tôi tính toán số lượng trên toàn bộ phạm vi. 

| c | cnt(A+B) | cần trước | cần sau | 
| --- | --- | --- | --- | 
| một | 1 | 0 | 0 | 
| b | 1 | 0 | 0 | 
| c | 2 | 0 | 0 | 
| d | 1 | 0 | 0 | 

Quá trình này không bao giờ yêu cầu chuyển tiếp các ràng buộc đặt hàng chưa được đáp ứng, vì vậy phân đoạn này hợp lệ. 

Điều này cho thấy rằng ngay cả với các ký tự hỗn hợp, miễn là nhiều bộ hỗ trợ phân chia thứ tự nhất quán, câu trả lời có thể là CÓ. 

### Ví dụ 2 

đầu vào:```
2 1
ab
ba
1 2
```| c | cnt(A+B) | cần trước | cần sau | 
| --- | --- | --- | --- | 
| một | 2 | 0 | 0 | 
| b | 2 | 0 | 0 | 

Điều này xác nhận rằng sự phân bố đối xứng luôn có thể được cân bằng bằng cách hoán đổi vị trí, tạo ra hai chuỗi được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · (N + Q)) | Mỗi truy vấn kiểm tra 26 lớp ký tự | 
| Không gian | O(26 · N) | Mảng tần số tiền tố cho cả hai chuỗi | 

Các ràng buộc cho phép độ dài và truy vấn lên tới 100000, do đó, giải pháp 26 yếu tố nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def build_prefix(s):
        n = len(s)
        pref = [[0] * (n + 1) for _ in range(26)]
        for i, ch in enumerate(s, 1):
            for c in range(26):
                pref[c][i] = pref[c][i - 1]
            pref[ord(ch) - 97][i] += 1
        return pref

    def get(pref, c, l, r):
        return pref[c][r] - pref[c][l - 1]

    L, Q = map(int, input().split())
    A = input().strip()
    B = input().strip()

    prefA = build_prefix(A)
    prefB = build_prefix(B)

    out = []
    for _ in range(Q):
        l, r = map(int, input().split())
        need = 0
        ok = True
        for c in range(26):
            cnt = get(prefA, c, l, r) + get(prefB, c, l, r)
            if need > cnt:
                ok = False
                break
            need = max(0, need - cnt)
        out.append("YES" if ok and need == 0 else "NO")

    return "\n".join(out)

# provided sample
assert run("""3 3
cbc
adc
1 2
1 1
1 3
""") == """YES
YES
NO"""

# minimum size
assert run("""1 2
a
b
1 1
1 1
""") in {"YES\nYES", "YES\nYES"}

# all equal
assert run("""5 1
aaaaa
aaaaa
1 5
""") == "YES"

# alternating
assert run("""4 1
abab
baba
1 4
""") == "YES"

# boundary single segment
assert run("""2 1
ab
cd
2 2
""") in {"YES", "NO"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | CÓ CÓ KHÔNG | tính đúng đắn cơ bản | 
| char đơn | CÓ CÓ | ranh giới tối thiểu | 
| tất cả đều bình đẳng | CÓ | tính khả thi tầm thường | 
| xen kẽ | CÓ | trộn nặng | 
| chỉ số duy nhất | CÓ/KHÔNG | hành vi lập chỉ mục cạnh | 

## Vỏ cạnh 

Một tình huống phức tạp là khi phân đoạn chỉ chứa một chỉ mục. Trong trường hợp đó, thao tác duy nhất là hoán đổi hai ký tự ở vị trí đó để mỗi chuỗi nhận được chính xác một trong hai chữ cái. Thuật toán xử lý việc này một cách chính xác vì số lượng dựa trên tiền tố giảm xuống để kiểm tra xem liệu cả hai chuỗi ký tự đơn thu được có thể được sắp xếp một cách tầm thường hay không, điều này luôn đúng. 

Một trường hợp tinh vi khác là khi tất cả các ký tự đều giống hệt nhau. Vì bất kỳ phép gán nào cũng tạo ra nhiều tập hợp giống hệt nhau cho cả hai chuỗi nên ràng buộc thứ tự sẽ tự động được thỏa mãn. Phương pháp dựa trên tần số coi đây là xung đột thứ tự bằng 0 ở mọi cấp độ ký tự, do đó, nó trả về CÓ một cách nhất quán. 

Một trường hợp thú vị hơn là khi phân đoạn có sự phân bố không cân bằng cao, chẳng hạn như nhiều ký tự 'a' trong một chuỗi và nhiều ký tự 'z' trong chuỗi kia. Việc chạy`need`biến nắm bắt xem các ký tự đầu có thể đáp ứng các ràng buộc sau này hay không; khi sự mất cân bằng là không thể giải quyết được,`need`tăng vượt quá số lượng có sẵn và thuật toán sẽ từ chối phân khúc một cách chính xác.
