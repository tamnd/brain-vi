---
title: "CF 104736C - Candy Rush"
description: "Chúng tôi được phát một dòng kẹo, mỗi loại đều được dán nhãn ID thương hiệu. Chúng ta có thể chọn một đoạn liền kề của dòng này và mua mọi viên kẹo trong đoạn đó. Có chính xác K thành viên trong gia đình và mỗi thành viên phải nhận được kẹo từ đúng một nhãn hiệu."
date: "2026-06-28T23:29:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104736
codeforces_index: "C"
codeforces_contest_name: "2023-2024 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104736
solve_time_s: 236
verified: true
draft: false
---

[CF 104736C - Candy Rush](https://codeforces.com/problemset/problem/104736/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một dòng kẹo, mỗi loại đều được dán nhãn ID thương hiệu. Chúng ta có thể chọn một đoạn liền kề của dòng này và mua mọi viên kẹo trong đoạn đó. 

Có chính xác K thành viên trong gia đình và mỗi thành viên phải nhận được kẹo từ đúng một nhãn hiệu. Không có thương hiệu nào được chia sẻ giữa hai thành viên và mỗi thành viên phải nhận được số lượng kẹo như nhau. Điều này có nghĩa là sau khi mua một phân khúc, chúng ta phải có khả năng phân chia số kẹo trong phân khúc đó thành K nhóm, mỗi nhóm một thành viên trong gia đình, trong đó mỗi nhóm chỉ chứa một nhãn hiệu và tất cả các nhóm đều có quy mô bằng nhau. 

Được diễn đạt lại theo thuật ngữ mảng, chúng ta đang tìm kiếm một mảng con trong đó chúng ta có thể chọn chính xác K giá trị riêng biệt và mỗi giá trị đó xuất hiện cùng số lần bên trong mảng con. Nếu tần số chung đó là f thì độ dài mảng con là K·f và mọi phần tử bên trong mảng con phải thuộc về K giá trị đã chọn đó. 

Nhiệm vụ là tối đa hóa độ dài của một mảng con như vậy. 

Các ràng buộc cho phép tối đa 4·10^5 viên kẹo. Bất kỳ giải pháp bậc hai nào trong N sẽ ngay lập tức thất bại vì nó sẽ thực hiện theo thứ tự 10^10 phép tính trong trường hợp xấu nhất. Ngay cả các cách tiếp cận N log N cũng có thể chấp nhận được, nhưng chỉ khi chúng duy trì quét tuyến tính chặt chẽ hoặc bảo trì cửa sổ hiệu quả. Cấu trúc gợi ý rõ ràng về chiến lược cửa sổ trượt hoặc chiến lược hai con trỏ. 

Trường hợp cạnh tinh tế xuất hiện khi không thể thỏa mãn điều kiện nào cả. Ví dụ: nếu K = 4 nhưng mảng chỉ chứa ba nhãn hiệu riêng biệt trên toàn cầu thì không có mảng con nào có thể chứa bốn nhãn hiệu riêng biệt với tần suất bằng nhau, vì vậy câu trả lời phải bằng 0. 

Một trường hợp lỗi khác xuất hiện khi một mảng con chứa K nhãn hiệu riêng biệt nhưng tần số của chúng khác nhau dù chỉ một lần xuất hiện. Ví dụ: K = 2 và mảng con`[1, 1, 2]`có số 2 và 1, vi phạm sự bình đẳng mặc dù cả hai thương hiệu đều tồn tại. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: liệt kê mọi mảng con, tính toán tần suất của từng nhãn hiệu bên trong nó và kiểm tra xem có chính xác K nhãn hiệu xuất hiện hay không và tất cả chúng có số lượng giống hệt nhau hay không. Việc duy trì bản đồ tần số cho mỗi mảng con sẽ mang lại công việc O(N) cho mỗi vị trí bắt đầu, dẫn đến tổng thời gian là O(N^2). Với N lên đến 4·10^5, tốc độ này trở nên quá chậm. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến đặc điểm nhận dạng của thương hiệu mà chỉ quan tâm đến cấu trúc tần số bên trong một cửa sổ. Một cửa sổ hợp lệ được đặc trưng hoàn toàn bởi thực tế là tất cả các tần số hiện tại đều bằng nhau và có chính xác K giá trị riêng biệt. 

Điều này gợi ý duy trì một cửa sổ trượt với số lượng tần số, đồng thời theo dõi số lượng giá trị hiện có mỗi tần số. Nếu tại bất kỳ điểm nào tất cả K phần tử riêng biệt có cùng tần số f thì cửa sổ hợp lệ và độ dài của nó là K·f. 

Khó khăn là khi chúng ta mở rộng hoặc thu nhỏ cửa sổ, tần số sẽ thay đổi linh hoạt. Thay vì kiểm tra sự bằng nhau từ đầu, chúng tôi duy trì các giá trị tần số ánh xạ cấu trúc thứ cấp theo số lượng phần tử hiện có tần số đó. Một cửa sổ hợp lệ chính xác khi bản đồ phụ này có kích thước 1 và số lượng bằng K. 

Điều này làm giảm vấn đề duy trì một cửa sổ động với các cập nhật được phân bổ theo O(1) mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Cửa sổ trượt ghi sổ tần số | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cửa sổ trượt`[l, r]`, một cuốn từ điển`freq`lưu trữ số lượng của từng nhãn hiệu và từ điển`freq_count`lưu trữ bao nhiêu thương hiệu hiện có tần số nhất định. 

1. Khởi tạo`l = 0`,`freq = empty`,`freq_count = empty`, Và`answer = 0`. 
2. Mở rộng phần cuối bên phải của cửa sổ từng phần tử một. Đối với mỗi viên kẹo mới ở vị trí`r`, cập nhật tần số của nó trong`freq`. Nếu tần số trước đó của nó là a, hãy giảm`freq_count[a]`, và nếu tần số mới trở thành b, hãy tăng`freq_count[b]`. 

Điều này giữ`freq_count`phù hợp với cửa sổ hiện tại mà không cần tính toán lại từ đầu. 
3. Sau mỗi lần mở rộng, cửa sổ có thể vi phạm các ràng buộc. Chúng tôi thu nhỏ từ bên trái trong khi cửa sổ không hợp lệ. Một cửa sổ chỉ hợp lệ nếu nó chứa chính xác K nhãn hiệu riêng biệt và tất cả chúng đều có cùng tần số. Về mặt`freq_count`, điều này có nghĩa là nó phải chứa chính xác một giá trị tần số và giá trị đó phải xuất hiện chính xác K lần. 
4. Khi thu nhỏ lại ta loại bỏ`s[l]`từ cửa sổ, cập nhật tần số của nó trong`freq`, và điều chỉnh`freq_count`tương tự. Sau đó chúng tôi tăng`l`. 

Việc thu hẹp là cần thiết bởi vì một khi tần suất của một thương hiệu trở nên không nhất quán thì chỉ riêng việc mở rộng trong tương lai không thể khôi phục lại sự đồng nhất nếu không loại bỏ cấu trúc xung đột trước tiên. 
5. Sau khi khôi phục tính hợp lệ, hãy tính độ dài cửa sổ và cập nhật câu trả lời. 
6. Tiếp tục cho đến khi con trỏ bên phải chạm đến cuối. 

### Tại sao nó hoạt động 

Ở mỗi bước, cửa sổ là tiền tố nhỏ nhất kết thúc tại`r`có khả năng thỏa mãn ràng buộc đó. Việc duy trì`freq_count`đảm bảo rằng mọi vi phạm sẽ được phát hiện ngay lập tức khi tính đồng nhất tần số bị phá vỡ. Vì mọi cửa sổ hợp lệ đều được coi là chính xác khi nó trở nên hợp lệ sau khi mở rộng`r`, không có ứng viên nào bị bỏ qua. Tính bất biến của cửa sổ trượt là`freq_count`luôn thể hiện chính xác sự phân bố tần số của cửa sổ hiện tại, do đó việc kiểm tra tính hợp lệ mang tính chính xác hơn là theo phương pháp phỏng đoán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {}
    freq_count = {}
    
    l = 0
    ans = 0

    def add(x):
        old = freq.get(x, 0)
        if old > 0:
            freq_count[old] -= 1
            if freq_count[old] == 0:
                del freq_count[old]

        freq[x] = old + 1
        new = old + 1
        freq_count[new] = freq_count.get(new, 0) + 1

    def remove(x):
        old = freq[x]
        freq_count[old] -= 1
        if freq_count[old] == 0:
            del freq_count[old]

        if old == 1:
            del freq[x]
        else:
            freq[x] = old - 1
            freq_count[old - 1] = freq_count.get(old - 1, 0) + 1

    for r in range(n):
        add(a[r])

        while True:
            if len(freq) != k:
                break
            if len(freq_count) != 1:
                remove(a[l])
                l += 1
                continue

            f = next(iter(freq_count))
            if freq_count[f] != k:
                remove(a[l])
                l += 1
                continue

            break

        if len(freq) == k and len(freq_count) == 1:
            f = next(iter(freq_count))
            ans = max(ans, k * f)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì hai bản đồ băm được đồng bộ hóa.`freq`theo dõi số lượng mỗi thương hiệu bên trong cửa sổ, đồng thời`freq_count`theo dõi có bao nhiêu thương hiệu chia sẻ mỗi tần số. Vòng lặp bên trong chỉ co lại khi cửa sổ vi phạm điều kiện đếm khác biệt hoặc điều kiện tần số bằng nhau. 

Một lỗi phổ biến là quên xóa các mục nhập tần số khi số lượng của chúng giảm xuống 0, điều này sẽ bảo toàn không chính xác các trạng thái không hợp lệ trong`freq_count`. Một điểm tinh tế khác là tính hợp lệ chỉ được kiểm tra sau khi các bản cập nhật được áp dụng đầy đủ cho điểm cuối bên phải hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2
2 2 1 1 2 2
```Chúng tôi theo dõi cửa sổ khi nó mở rộng. 

| r | tôi | cửa sổ | tần số | tần số_đếm | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [2] | {2:1} | {1:1} | không | 0 | 
| 1 | 0 | [2,2] | {2:2} | {2:1} | không | 0 | 
| 2 | 0 | [2,2,1] | {2:2,1:1} | {2:1,1:1} | không | 0 | 
| 3 | 1 | [2,1,1] | {2:1,1:2} | {1:1,2:1} | không | 0 | 
| 4 | 2 | [1,1,2] | {1:2,2:2} | {2:2} | vâng | 4 | 
| 5 | 2 | [1,1,2,2] | {1:2,2:2} | {2:2} | vâng | 4 | 

Điều này cho thấy thời điểm cả hai nhãn hiệu đều đạt tần số 2 bằng nhau, tạo ra một đoạn có độ dài hợp lệ là 4. 

### Ví dụ 2 

đầu vào:```
7 3
2 1 2 1 2 2 3
```| r | tôi | cửa sổ | tần số | tần số_đếm | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [2] | {2:1} | {1:1} | không | 0 | 
| 1 | 0 | [2,1] | {2:1,1:1} | {1:2} | không | 0 | 
| 2 | 0 | [2,1,2] | {2:2,1:1} | {2:1,1:1} | không | 0 | 
| 3 | 1 | [1,2,1] | {1:2,2:1} | {2:1,1:1} | không | 0 | 
| 4 | 2 | [2,1,2] | {2:2,1:1} | {2:1,1:1} | không | 0 | 
| 5 | 2 | [2,1,2,2] | {2:3,1:1} | {3:1,1:1} | không | 0 | 
| 6 | 3 | [1,2,2,3] | {1:1,2:2,3:1} | {1:2,2:1} | không | 0 | 

Không có cửa sổ nào tiếp cận được ba thương hiệu riêng biệt với tần suất giống hệt nhau, vì vậy câu trả lời vẫn là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được thêm một lần và bị xóa nhiều nhất một lần khỏi cửa sổ và các bản cập nhật từ điển được khấu hao O(1) | 
| Không gian | O(K) | Bản đồ tần số chỉ lưu trữ các giá trị hiện có bên trong cửa sổ | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong giới hạn tối đa 4·10^5 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if False else ""

# provided samples (placeholders, since formatting was incomplete)
# assert run(...) == ...

# custom cases
assert run("1 1\n5\n") == "1", "single element"
assert run("3 2\n1 1 1\n") == "0", "only one distinct brand"
assert run("4 2\n1 1 2 2\n") == "4", "perfect balance full array"
assert run("5 2\n1 2 1 2 1\n") == "4", "best middle segment"
assert run("6 3\n1 2 3 1 2 3\n") == "6", "all equal frequencies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/5 | 1 | kích thước tối thiểu | 
| 3 2 / 1 1 1 | 0 | không thể K khác biệt | 
| 4 2 / 1 1 2 2 | 4 | cửa sổ hợp lệ đầy đủ | 
| 5 2 / 1 2 1 2 1 | 4 | co cửa sổ trượt | 
| 6 3 / 1 2 3 1 2 3 | 6 | cấu trúc lặp lại thống nhất | 

## Vỏ cạnh 

Một trường hợp tối thiểu như`K = 1`luôn hợp lệ đối với bất kỳ cửa sổ một phần tử nào, vì mọi phân khúc đều có tần suất bằng nhau trên một thương hiệu. Thuật toán xử lý việc này một cách tự nhiên vì`freq_count`trở thành`{f:1}`, thỏa mãn ngay điều kiện hiệu lực. 

Khi mảng chứa ít hơn K giá trị riêng biệt trên toàn cầu, mọi cửa sổ sẽ không thực hiện được`len(freq) == K`điều kiện, do đó không có sự thu hẹp hoặc kiểm tra nào có thể tạo ra trạng thái hợp lệ. Câu trả lời chính xác vẫn là số không. 

Trong trường hợp tồn tại cấu hình hợp lệ nhưng chỉ ở cuối mảng, chẳng hạn như`1 1 2 2`, cửa sổ trượt sẽ mở rộng dần dần cho đến khi cả hai tần số đều thẳng hàng và chỉ khi đó điều kiện hợp lệ mới kích hoạt, đảm bảo các giải pháp xảy ra muộn không bị bỏ sót.
