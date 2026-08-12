---
title: "CF 104027H - \u8fd8\u662f\u5206\u7cd6\u679c"
description: "Chúng ta có hai chuỗi, gọi chúng là A và B. Mỗi truy vấn cung cấp cho chúng ta hai tiền tố: A[1..x] và B[1..y]. Nhiệm vụ là quyết định xem hai tiền tố này có “khớp nhau về các phần tử riêng biệt” theo cách đối xứng hay không: mọi giá trị xuất hiện trong tiền tố của A phải xuất hiện trong…"
date: "2026-07-02T04:09:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104027
codeforces_index: "H"
codeforces_contest_name: "The 10-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 104027
solve_time_s: 44
verified: true
draft: false
---

[CF 104027H - \u8fd8\u662f\u5206\u7cd6\u679c](https://codeforces.com/problemset/problem/104027/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi, gọi chúng là A và B. Mỗi truy vấn cung cấp cho chúng ta hai tiền tố: A[1..x] và B[1..y]. Nhiệm vụ là quyết định xem hai tiền tố này có “khớp nhau về các phần tử riêng biệt” theo cách đối xứng hay không: mọi giá trị xuất hiện trong tiền tố của A phải xuất hiện trong tiền tố của B và mọi giá trị xuất hiện trong tiền tố của B phải xuất hiện trong tiền tố của A. 

Tương tự, nếu chúng ta nén từng tiền tố vào tập hợp các giá trị riêng biệt mà nó chứa, truy vấn sẽ hỏi liệu hai tập hợp này có giống nhau hay không. 

Điểm tinh tế là các bản sao không quan trọng chút nào. Chỉ sự tồn tại của mỗi giá trị bên trong tiền tố mới quan trọng chứ không phải số lần nó xuất hiện. 

Từ góc độ phức tạp, các ràng buộc tự nhiên đối với loại vấn đề này đủ lớn đến mức không thể tính toán lại các tập hợp cho mỗi truy vấn. Nếu có n phần tử và q truy vấn, thì bất kỳ phương pháp nào quét tiền tố trên mỗi truy vấn đều dẫn đến O(nq), vượt xa các giới hạn thông thường. Ngay cả việc duy trì các bộ cho mỗi truy vấn một cách độc lập cũng sẽ xuống cấp nhanh chóng nếu được thực hiện một cách ngây thơ. 

Điểm tinh tế thứ hai là các giá trị vốn không nhỏ hoặc liền kề nhau. Bất kỳ giải pháp nào dựa trên mảng tần số trực tiếp mà không nén hoặc băm trước tiên phải chuẩn hóa các giá trị hoặc sử dụng bản đồ, điều này có thể trở thành nút cổ chai nếu thực hiện nhiều lần. 

Một trường hợp lỗi điển hình xuất hiện khi một bên chứa các bản sao mà bên kia không có hoặc khi các phần tử xuất hiện theo thứ tự khác nhau nhưng có bộ giống hệt nhau. 

Ví dụ: giả sử A = [1, 2, 1, 3] và B = [1, 3, 2]. Với x = 4, y = 3, cả hai tiền tố đều chứa chính xác {1, 2, 3}, nên câu trả lời là CÓ. Việc triển khai bất cẩn khi so sánh các tổng tiền tố hoặc tần số thay vì các tập hợp riêng biệt sẽ coi điều này là khác nhau một cách không chính xác. 

Một trường hợp khác là khi một tiền tố nhỏ hơn hoàn toàn nhưng đã chứa tất cả các phần tử riêng biệt của tiền tố kia. Ví dụ: A = [1, 2, 3, 3, 3], B = [1, 2, 3, 4]. Tại x = 5, y = 3, A có {1, 2, 3}, B có {1, 2, 3, 4} nên câu trả lời là KHÔNG mặc dù hầu hết các phần tử đều trùng nhau. 

Khó khăn cốt lõi là duy trì các bộ tiền tố một cách hiệu quả trong nhiều truy vấn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng tập hợp riêng biệt cho mọi tiền tố của A và mọi tiền tố của B. Đối với mỗi tiền tố, chúng tôi lưu trữ tập hợp các phần tử đã thấy cho đến nay, sau đó đối với mỗi truy vấn, chúng tôi so sánh hai bộ. 

Điều này đúng, nhưng việc xây dựng các bộ cho mỗi tiền tố và so sánh chúng cho mỗi truy vấn là rất tốn kém. Ngay cả khi các so sánh tập hợp được tối ưu hóa, mỗi so sánh có thể tiêu tốn thời gian tuyến tính theo số lượng phần tử riêng biệt trong tiền tố. Trong trường hợp xấu nhất khi tất cả các phần tử đều khác biệt, mỗi truy vấn có chi phí O(n), dẫn đến tổng thời gian là O(nq). 

Quan sát quan trọng là điều quan trọng đối với tiền tố không phải là cấu trúc tập hợp đầy đủ mà là liệu hai tiền tố có chứa chính xác các phần tử riêng biệt giống nhau hay không. Điều này gợi ý biến mỗi tiền tố thành một dấu vân tay nhỏ gọn. Nếu hai tiền tố có bộ giống hệt nhau thì dấu vân tay của chúng phải khớp nhau. Nếu chúng ta có thể duy trì biểu diễn cuộn của tập hợp các phần tử riêng biệt, chúng ta có thể trả lời từng truy vấn trong O(1). 

Một cách tiêu chuẩn để thực hiện điều này là gán cho mỗi giá trị riêng biệt một hàm băm ngẫu nhiên và duy trì, đối với mỗi tiền tố, XOR của các hàm băm của tất cả các phần tử riêng biệt được thấy cho đến nay. Bí quyết quan trọng là không được tính các bản sao hai lần, vì vậy chúng tôi theo dõi xem phần tử đã xuất hiện trong tiền tố hay chưa. Khi lần đầu tiên nhìn thấy một phần tử, chúng tôi XOR hàm băm của nó thành giá trị tiền tố; những lần xuất hiện sau đó sẽ bị bỏ qua. 

Điều này biến đổi mỗi tiền tố thành một số nguyên duy nhất (hoặc cặp số nguyên để đảm bảo an toàn) và đẳng thức của các tập hợp trở thành đẳng thức của các giá trị băm. Sau đó, mỗi truy vấn sẽ giảm xuống mức so sánh hai giá trị băm tiền tố được tính toán trước.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đặt cho mỗi truy vấn) | O(nq) | O(n) | Quá chậm | 
| Tiền tố băm riêng biệt | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp riêng cho A và B. 

1. Gán mỗi giá trị có thể một hàm băm số nguyên 64 bit ngẫu nhiên. Điều này phục vụ như một đóng góp dấu vân tay duy nhất cho giá trị đó. 
2. Quét mảng A từ trái sang phải trong khi vẫn duy trì mảng boolean hoặc bản đồ băm cho biết giá trị đã xuất hiện hay chưa. Duy trì giá trị XOR đang chạy`hashA[i]`biểu diễn tập hợp các phần tử phân biệt trong A[1..i]. Nếu A[i] được nhìn thấy lần đầu tiên, XOR hàm băm của nó thành giá trị đang chạy. 
3. Lặp lại quy trình tương tự cho mảng B, tạo ra`hashB[j]`. 
4. Với mỗi truy vấn (x, y), so sánh`hashA[x]`Và`hashB[y]`. Nếu chúng bằng nhau thì xuất ra CÓ, ngược lại thì KHÔNG. 

Lựa chọn thiết kế quan trọng là bỏ qua những lần xuất hiện lặp đi lặp lại. Nếu không có điều này, XOR sẽ loại bỏ các bản sao một cách không chính xác và phá vỡ tính chính xác. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào, mỗi giá trị đóng góp chính xác một lần khi và chỉ khi nó xuất hiện ít nhất một lần trong tiền tố đó. Sự tích lũy XOR hoạt động giống như một chỉ báo đã đặt vì XOR có tính giao hoán và tự nghịch đảo. Vì chúng tôi chỉ chèn một giá trị vào lần đầu tiên nó xuất hiện nên hàm băm tiền tố chính xác là XOR của hàm băm của tất cả các phần tử riêng biệt trong tiền tố đó. Hai tiền tố có các tập hợp riêng biệt giống hệt nhau khi và chỉ khi chúng XOR cùng nhiều tập hợp băm, tạo ra cùng một kết quả với xác suất cao khi sử dụng các hàm băm ngẫu nhiên đủ lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    import random
    rnd = random.getrandbits

    # assign hash per value
    H = {}

    def get_hash(x):
        if x not in H:
            H[x] = rnd(64)
        return H[x]

    seenA = set()
    seenB = set()

    prefA = [0] * (n + 1)
    prefB = [0] * (m + 1)

    cur = 0
    for i in range(1, n + 1):
        v = A[i - 1]
        if v not in seenA:
            seenA.add(v)
            cur ^= get_hash(v)
        prefA[i] = cur

    cur = 0
    for i in range(1, m + 1):
        v = B[i - 1]
        if v not in seenB:
            seenB.add(v)
            cur ^= get_hash(v)
        prefB[i] = cur

    out = []
    for _ in range(q):
        x, y = map(int, input().split())
        out.append("YES" if prefA[x] == prefB[y] else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng các biểu diễn tiền tố cho cả hai mảng một cách độc lập. Chi tiết triển khai quan trọng là`seenA`Và`seenB`bộ. Nếu không có chúng, các lần xuất hiện lặp lại sẽ chuyển đổi không chính xác các giá trị XOR, phá vỡ hành vi “chỉ đặt”. 

Một cách tinh tế khác là sử dụng phép gán băm ngẫu nhiên dựa trên từ điển thay vì dựa vào hàm băm tích hợp của Python, được thêm muối cho mỗi quy trình và không ổn định trong các lần chạy theo cách được kiểm soát. Số nguyên 64 bit ngẫu nhiên rõ ràng đảm bảo khả năng tái tạo. 

Cuối cùng, mảng tiền tố được lập chỉ mục 1 để đơn giản hóa việc xử lý truy vấn và tránh việc thay đổi chỉ mục lặp lại. 

## Ví dụ đã hoạt động 

Xét A = [1, 2, 1, 3], B = [2, 3, 1] và một truy vấn (4, 3). 

Chúng tôi theo dõi băm tiền tố: 

| tôi | A[i] | đã thấyA | trướcA | 
| --- | --- | --- | --- | 
| 1 | 1 | {1} | h(1) | 
| 2 | 2 | {1,2} | h(1) ⊕ h(2) | 
| 3 | 1 | {1,2} | h(1) ⊕ h(2) | 
| 4 | 3 | {1,2,3} | h(1) ⊕ h(2) ⊕ h(3) | 

Tương tự với B: 

| j | B[j] | đã thấyB | trướcB | 
| --- | --- | --- | --- | 
| 1 | 2 | {2} | h(2) | 
| 2 | 3 | {2,3} | h(2) ⊕ h(3) | 
| 3 | 1 | {1,2,3} | h(1) ⊕ h(2) ⊕ h(3) | 

Ở truy vấn (4,3), cả hai giá trị băm đều khớp nhau nên câu trả lời là CÓ. Điều này chứng tỏ rằng thứ tự và sự lặp lại không ảnh hưởng đến kết quả. 

Bây giờ hãy xem xét A = [1,1,2], B = [1,2,2,3], truy vấn (3,3). 

| Tiền tố A | đặt | băm | 
| --- | --- | --- | 
| 1..3 | {1,2} | h(1) ⊕ h(2) | 

| Tiền tố B | đặt | băm | 
| --- | --- | --- | 
| 1..3 | {1,2,3} | h(1) ⊕ h(2) ⊕ h(3) | 

Sự không khớp chính xác tạo ra NO, cho thấy độ nhạy đối với các phần tử bị thiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q) | Mỗi phần tử được xử lý một lần để xây dựng các giá trị băm tiền tố và mỗi truy vấn được trả lời theo thời gian không đổi | 
| Không gian | O(n + m) | Mảng tiền tố lưu trữ một hàm băm cho mỗi vị trí, cộng với bản đồ hàm băm để ánh xạ giá trị | 

Các ràng buộc thường liên quan đến các vấn đề truy vấn tiền tố làm cho độ phức tạp này trở nên an toàn vì nó thay đổi tuyến tính theo kích thước đầu vào và tránh mọi thao tác quét tiền tố theo mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample-like case
assert run("3 3 2\n1 2 3\n3 2 1\n2 2\n3 3\n") in {"YES\nYES", "YES\nYES".lower()}

# identical arrays
assert run("4 4 1\n1 2 3 4\n1 2 3 4\n4 4\n") == "YES"

# different sets
assert run("3 3 1\n1 2 3\n1 2 4\n3 3\n") == "NO"

# duplicates inside A
assert run("5 3 1\n1 1 1 2 3\n1 2 3\n5 3\n") == "YES"

# minimal case
assert run("1 1 1\n7\n7\n1 1\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng giống hệt nhau | CÓ | sự bình đẳng của các tiền tố đầy đủ | 
| bộ khác nhau | KHÔNG | phát hiện phần tử bị thiếu | 
| trùng lặp bên trong A | CÓ | trùng lặp bị bỏ qua một cách chính xác | 
| trường hợp tối thiểu | CÓ | xử lý ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là sự lặp lại nhiều. Với A = [5,5,5,5] và B = [5], mọi tiền tố của A sẽ hoạt động giống hệt nhau sau lần xuất hiện đầu tiên. Thuật toán xử lý việc này vì`seenA`ngăn chặn việc chuyển đổi XOR lặp đi lặp lại, do đó hàm băm tiền tố sẽ ổn định ngay sau 5 lần đầu tiên. 

Một trường hợp cạnh khác là khi các phần tử được xen kẽ khác nhau nhưng các bộ đều khớp nhau. Đối với A = [1,2,1,3,2] và B = [3,2,1], giá trị băm tiền tố cuối cùng phải khớp ngay cả khi B ngắn hơn và không có thứ tự. Thuật toán giảm chính xác cả hai thành XOR(h(1), h(2), h(3)). 

Trường hợp cạnh thứ ba là các truy vấn lặp lại có (x, y) giống hệt nhau. Vì câu trả lời chỉ phụ thuộc vào mảng được tính toán trước nên các truy vấn lặp lại không tốn thêm thay đổi trạng thái và luôn trả về kết quả nhất quán mà không cần tính toán lại.
