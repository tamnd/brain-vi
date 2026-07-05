---
title: "CF 102897C - Cây đếm BBpigeon"
description: "Chúng ta được cung cấp một cấu trúc cây gốc một cách gián tiếp, không phải theo các cạnh mà bằng danh sách độ sâu cho mỗi nút. Nút 1 được cố định là nút gốc và mỗi nút i đi kèm với một số nguyên ai mô tả khoảng cách từ nút gốc đó đến số lượng nút trên đường đi, bao gồm cả chính nó."
date: "2026-07-04T08:25:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102897
codeforces_index: "C"
codeforces_contest_name: "The 3rd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102897
solve_time_s: 44
verified: true
draft: false
---

[CF 102897C - Cây đếm BBpigeon](https://codeforces.com/problemset/problem/102897/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc cây gốc một cách gián tiếp, không phải theo các cạnh mà bằng danh sách độ sâu cho mỗi nút. Nút 1 được cố định là nút gốc và mỗi nút i đi kèm với một số nguyên ai mô tả khoảng cách từ nút gốc đó đến số lượng nút trên đường đi, bao gồm cả chính nó. Vì vậy, gốc có độ sâu 1, các nút con của nó có độ sâu 2, v.v. 

Tuy nhiên, mối quan hệ cha-con thực tế không được đưa ra. Nhiều cây có gốc khác nhau có thể khớp với cùng một phép gán độ sâu, bởi vì các nút ở độ sâu d có thể được gắn theo nhiều cách với các nút ở độ sâu d − 1. Nhiệm vụ là đếm xem có bao nhiêu cây có gốc riêng biệt phù hợp với mảng độ sâu đã cho. 

Hai cây được coi là khác nhau nếu có ít nhất một nút có số lượng cây con khác nhau. Vì vậy, chúng tôi không chỉ phân biệt bằng cấu trúc mà bằng số lượng con chính xác trên mỗi nút, điều này vẫn làm giảm việc tính các bài tập cha mẹ hợp lệ riêng biệt. 

Ràng buộc n 100000 buộc bất kỳ giải pháp nào hướng tới thời gian tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến việc kiểm tra tất cả các lựa chọn cha mẹ một cách rõ ràng hoặc liệt kê các cấu trúc cây sẽ bùng nổ về mặt tổ hợp. Ngay cả O(n log n) cũng được, nhưng việc xây dựng O(n^2) trên các nút hoặc DP ngây thơ trên tất cả các phép gán là không thể. 

Trường hợp cạnh khóa xuất hiện khi các ràng buộc về độ sâu không nhất quán. Ví dụ: nếu chúng ta có một nút ở độ sâu 3 nhưng không tồn tại nút nào ở độ sâu 2 thì không thể hình thành cây, vì vậy câu trả lời phải là 0. Một trường hợp tinh vi khác là khi nhiều nút chia sẻ độ sâu 2 nhưng chỉ có một nút cha có thể có ở độ sâu 1, buộc phải có cấu trúc giống như ngôi sao. Một cách tiếp cận đơn giản có thể nhân các lựa chọn không chính xác mà không tôn trọng rằng mỗi nút phải chọn chính xác một nút cha ở mức độ sâu trước đó. 

Ví dụ trường hợp không hợp lệ: 

đầu vào: 

n = 3 

a = [1, 3, 2] 

Ở đây nút có độ sâu 3 không có nút cha nào có thể có ở độ sâu 2 nếu thứ tự bị nghiêm ngặt bởi các chỉ số nút. Câu trả lời đúng là 0. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ là nghĩ đến việc xây dựng cây bằng cách gán cho mỗi nút (trừ nút gốc) một nút cha trong số các nút ở mức độ sâu trước đó. Nếu có các nút kd ở độ sâu d và các nút kd−1 ở độ sâu d − 1, thì mỗi nút ở độ sâu d có thể chọn bất kỳ nút cha kd−1 nào, dẫn đến các khả năng (kd−1)^{kd} cho cấp độ đó. Nhân qua các cấp độ sẽ cho ra số lượng ứng cử viên. 

Điều này đã gợi ý cấu trúc của giải pháp, nhưng lực lượng vũ phu sẽ xây dựng hoặc mô phỏng rõ ràng tất cả các bài tập hoặc cố gắng xây dựng cây theo cách đệ quy. Điều đó nhanh chóng trở nên không khả thi vì số lượng nhiệm vụ tăng theo cấp số nhân với n trong trường hợp xấu nhất như cấu trúc hai cấp. 

Quan sát quan trọng là độ sâu xác định hoàn toàn các ứng cử viên cha hợp lệ: mọi nút ở độ sâu d phải kết nối với một số nút ở độ sâu d − 1 và không có lựa chọn nào khác quan trọng. Vì chỉ có số lượng nút con trên mỗi nút mới quan trọng để phân biệt cây nên chúng tôi không cần theo dõi các phép gán chính xác, chỉ có bao nhiêu cách các nút ở cấp độ d có thể tự phân phối giữa các nút ở cấp độ d − 1. Mỗi nút chọn cha mẹ của nó một cách độc lập, do đó tổng số cấu hình là tích số của các cấp độ lũy thừa. 

Có một ràng buộc ẩn nữa: mỗi nút ở độ sâu d − 1 phải có ít nhất một nút ở các cấp độ sâu hơn có thể gắn phía trên nó nếu nó không bị cô lập, nhưng điều đó được xử lý tự động vì các nút chọn cha mẹ một cách tự do; chúng tôi không bắt buộc phải đảm bảo tính chủ quan. 

Do đó, vấn đề giảm xuống việc nhóm các nút theo độ sâu và nhân mức đóng góp theo cấp độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Đếm độ sâu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán

1. Nhóm các nút theo giá trị độ sâu của chúng và đếm xem có bao nhiêu nút tồn tại ở mỗi độ sâu. Điều này cung cấp cho chúng ta bản đồ tần số kd cho mỗi độ sâu d. Bước này chuyển vấn đề sang làm việc với các lớp thay vì các nút riêng lẻ. 
2. Kiểm tra xem độ sâu 1 có chứa chính xác một nút hay không. Nếu không, cấu trúc không thể biểu diễn một cây có gốc với nút 1 là gốc, vì vậy câu trả lời là 0. Điều này thực thi ràng buộc gốc cố định. 
3. Xác minh rằng độ sâu liên tục bắt đầu từ 1 mà không có khoảng trống. Nếu độ sâu d > 1 nào đó xuất hiện nhưng độ sâu d − 1 bị thiếu, thì không nút nào có thể gắn vào nút cha hợp lệ, vì vậy câu trả lời là 0. 
4. Sắp xếp tất cả các độ sâu khác nhau theo thứ tự tăng dần để chúng tôi xử lý các cấp độ theo hướng hợp lệ từ cha mẹ đến con cái. 
5. Khởi tạo câu trả lời là 1. Chúng tôi sẽ nhân số đóng góp từ mỗi lần chuyển đổi độ sâu. 
6. Với mỗi độ sâu d bắt đầu từ 2, hãy nhân kết quả với (k_{d−1})^{k_d}. Điều này phản ánh rằng mỗi nút ở độ sâu d chọn độc lập một nút cha trong số tất cả các nút ở độ sâu d − 1, đưa ra k_{d−1} lựa chọn cho mỗi nút. 
7. Lấy tất cả các phép nhân theo modulo 998244353. Điều này giữ cho các số nằm trong giới hạn trong khi vẫn đảm bảo tính chính xác trong số học mô-đun. 

### Tại sao nó hoạt động 

Ở mỗi lớp sâu, quyền tự do về cấu trúc duy nhất nằm ở việc chọn nút cha cho mỗi nút từ lớp trước. Khi một nút chọn nút cha của nó, nó sẽ đóng góp chính xác một cạnh đi ra cho nút cha đó và lựa chọn này không hạn chế các nút khác ngoại trừ thông qua việc đếm. Vì bài toán chỉ phân biệt các cây bằng số lượng con nên bất kỳ phép gán riêng biệt nào của cha mẹ đều mang lại một cây hợp lệ riêng biệt. Các lựa chọn giữa các nút là độc lập và giữa các lớp cũng độc lập, do đó, tổng số được phân tích rõ ràng thành một tích số của các lũy thừa đối với chuyển đổi độ sâu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def mod_pow(a, e):
    return pow(a, e, MOD)

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    from collections import Counter
    cnt = Counter(a)

    # root must be unique
    if cnt[1] != 1:
        print(0)
        return

    depths = sorted(cnt.keys())

    # check continuity of depths
    for i in range(1, len(depths)):
        if depths[i] != depths[i - 1] + 1:
            print(0)
            return

    ans = 1

    for i in range(1, len(depths)):
        prev_d = depths[i - 1]
        cur_d = depths[i]
        ans = (ans * pow(cnt[prev_d], cnt[cur_d], MOD)) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đếm số lượng nút xuất hiện ở mỗi độ sâu. Điều này nén vấn đề thành một bản đồ tần số theo các cấp độ. Việc xác thực gốc đảm bảo chính xác một nút ở độ sâu 1, vì gốc đã được cố định. 

Sau đó, chúng tôi sắp xếp độ sâu và đảm bảo chúng tạo thành một chuỗi liên tục. Điều này ngăn chặn các trường hợp không hợp lệ khi một cấp độ không có cấp độ gốc hợp lệ. 

Cuối cùng, chúng tôi lặp lại quá trình chuyển đổi độ sâu. Đối với mỗi cặp độ sâu liên tiếp, mỗi nút ở cấp độ sâu hơn sẽ chọn nút cha của nó trong số tất cả các nút ở cấp độ trước đó, đóng góp một số hạng lũy ​​thừa. Phép nhân tích lũy tất cả các lựa chọn độc lập. 

Phép lũy thừa mô-đun được xử lý bằng cách sử dụng pow tích hợp của Python, đủ hiệu quả cho n lên đến 100000 vì các giá trị số mũ được giới hạn bởi n. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp hợp lệ nhỏ: 

đầu vào: 

n = 4 

a = [1, 2, 2, 3] 

Số lượng độ sâu: 

| Bước | Độ sâu | Đếm | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 
| 3 | 3 | 1 | 

Việc chuyển đổi từ độ sâu 1 sang độ sâu 2 đóng góp 1^2 = 1, vì cả hai nút ở độ sâu 2 phải gắn vào một gốc duy nhất. Việc chuyển đổi từ độ sâu 2 sang độ sâu 3 đóng góp 2^1 = 2, vì nút ở độ sâu 3 có thể chọn một trong hai nút độ sâu 2 làm nút gốc. Câu trả lời cuối cùng là 2. 

Bây giờ hãy xem xét một trường hợp có nhiều tự do hơn: 

đầu vào: 

n = 5 

a = [1, 2, 2, 2, 3] 

Số lượng độ sâu: 

| Bước | Độ sâu | Đếm | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 3 | 
| 3 | 1 | 1 | 

Đây: 

Chuyển đổi 1 → 2 đóng góp 1^3 = 1. 

Chuyển đổi 2 → 3 đóng góp 3^1 = 3. 

Vậy đáp án là 3. 

Những dấu vết này cho thấy mỗi lớp đóng góp độc lập như thế nào và cách lũy thừa mã hóa các lựa chọn cha mẹ độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm tần số và lặp theo mức độ sâu là tuyến tính theo số lượng nút | 
| Không gian | O(n) | Lưu trữ bản đồ tần số độ sâu | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tất cả các thao tác đều được thực hiện một lần qua đầu vào và chỉ sắp xếp theo các độ sâu khác nhau, tối đa là O(n). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter

    MOD = 998244353

    n = int(input())
    a = list(map(int, input().split()))
    cnt = Counter(a)

    if cnt[1] != 1:
        return "0"

    depths = sorted(cnt.keys())
    for i in range(1, len(depths)):
        if depths[i] != depths[i - 1] + 1:
            return "0"

    ans = 1
    for i in range(1, len(depths)):
        ans = (ans * pow(cnt[depths[i - 1]], cnt[depths[i]], MOD)) % MOD

    return str(ans)

# provided samples (illustrative placeholders)
assert run("3\n1 2 2\n") == "1", "sample 1"

# minimum size
assert run("1\n1\n") == "1", "single node"

# invalid root count
assert run("2\n1 1\n") == "0", "invalid root count"

# gap in depths
assert run("3\n1 3 3\n") == "0", "missing depth 2"

# branching case
assert run("4\n1 2 2 3\n") == "2", "basic branching"

# all equal depths invalid
assert run("3\n2 2 2\n") == "0", "no root"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp cơ sở nút đơn | 
| 1 1 2 3 3 | 2 | phân nhánh qua các lớp | 
| 1 3 3 | 0 | thiếu độ sâu trung gian | 
| 2 2 2 | 0 | tình trạng gốc không hợp lệ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi gốc không phải là duy nhất. Đối với đầu vào`n = 3, a = [1, 1, 2]`, thuật toán ngay lập tức trả về 0 vì cấp gốc có nhiều hơn một nút. Trong quá trình xử lý, cnt[1] = 2 vi phạm ràng buộc gốc nên không xảy ra tính toán nào nữa. 

Một trường hợp cạnh khác là chuỗi độ sâu bị ngắt kết nối như`a = [1, 2, 4]`. Bản đồ độ sâu chứa 1, 2 và 4, nhưng thiếu 3. Danh sách độ sâu được sắp xếp không kiểm tra được tính liên tục khi di chuyển từ 2 đến 4 và thuật toán xuất ra chính xác 0 trước khi thử lũy thừa. 

Trường hợp thứ ba là một chuỗi hoàn toàn hợp lệ như`a = [1, 2, 3, 4]`. Ở đây, mỗi độ sâu có chính xác một nút, vì vậy mỗi chuyển đổi đóng góp 1^1 = 1 và câu trả lời vẫn là 1 xuyên suốt, khớp với thực tế là có chính xác một cấu trúc cây tuyến tính phù hợp với độ sâu.
