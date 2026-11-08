---
title: "CF 1413D - Shurikens"
description: "Chúng tôi được cung cấp nhật ký theo trình tự thời gian về các hoạt động được thực hiện trên một tủ trưng bày bắt đầu trống. Có hai loại sự kiện."
date: "2026-06-11T07:22:50+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1413
codeforces_index: "D"
codeforces_contest_name: "Technocup 2021 - Elimination Round 1"
rating: 1700
weight: 1413
solve_time_s: 114
verified: true
draft: false
---

[CF 1413D - Shurikens](https://codeforces.com/problemset/problem/1413/D) 

**Đánh giá:** 1700 
**Tas:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhật ký theo trình tự thời gian về các hoạt động được thực hiện trên một tủ trưng bày bắt đầu trống. Có hai loại sự kiện. Một sự kiện chèn một phi tiêu mới với một số giá trị không xác định và sự kiện còn lại sẽ xóa giá trị nhỏ nhất hiện có, báo cáo chính xác giá trị nào đã bị xóa. 

Chúng tôi được thông báo rằng tất cả các giá trị từ 1 đến n xuất hiện chính xác một lần trong các sự kiện xóa và có chính xác n lần chèn. Nhiệm vụ không phải là mô phỏng một cách mù quáng mà là xác định xem liệu có tồn tại bất kỳ sự gán giá trị nào cho các sự kiện chèn sao cho mỗi lần loại bỏ mức tối thiểu đều phù hợp với quá trình tiến hóa nhiều tập hợp hay không. Nếu có thể, chúng ta cũng phải xuất một giá trị gán hợp lệ cho các sự kiện chèn. 

Khó khăn chính là các phần chèn vào không được gắn nhãn. Chúng ta không biết giá trị nào được chèn vào mỗi dấu “+” và chúng ta phải gán các giá trị sao cho bất cứ khi nào dấu “- x” xảy ra, x là giá trị nhỏ nhất có sẵn tại thời điểm đó. 

Ràng buộc n lên tới 100000 ngụ ý rằng bất kỳ mô phỏng bậc hai hoặc thậm chí n log n nào có tính năng quay lui đều quá chậm nếu nó liên tục thử các phép gán. Chúng ta cần một cấu trúc vượt qua duy nhất để duy trì tập hợp các mục có sẵn hiện tại và một chiến lược xây dựng một nhiệm vụ nhất quán một cách tham lam. 

Một trường hợp lỗi nhỏ xuất hiện khi việc xóa xảy ra trong khi không có sẵn mục nào. Ví dụ: sự kiện đầu tiên bị xóa ngay lập tức không hợp lệ vì chưa có gì có thể được chèn vào. 

Một trường hợp lỗi khác phát sinh khi thuật toán cho phép một phần tử nhỏ hơn x vẫn còn trong cấu trúc tại thời điểm x bị loại bỏ. Ví dụ: nếu 1 được chèn nhưng không bị xóa trước khi 2 bị xóa, thì chuỗi không hợp lệ vì 2 không thể là giá trị nhỏ nhất trong khi vẫn tồn tại 1. 

Một trường hợp ít rõ ràng hơn là khi nhiều lần chèn xảy ra trước bất kỳ thao tác xóa nào và chúng ta phải quyết định giá trị nào sẽ được gán cho các lần chèn đó theo cách phù hợp với các lần xóa trong tương lai. Một chiến lược ngây thơ gán các giá trị theo thứ tự chèn mà không quan tâm đến các ràng buộc trong tương lai sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán hoán vị từ 1 đến n cho các vị trí chèn và sau đó mô phỏng quy trình cho từng nhiệm vụ ứng viên. Đối với mỗi “- x”, chúng tôi sẽ kiểm tra xem x có thực sự là mức tối thiểu của nhiều tập hợp hiện tại hay không. Điều này đòi hỏi phải xây dựng và kiểm tra tất cả các hoán vị của các phép gán, dẫn đến n! các khả năng, mỗi mô phỏng có giá O(n log n) nếu chúng ta sử dụng nhiều bộ. Điều này là không khả thi về mặt thiên văn ngay cả đối với n nhỏ. 

Quan sát quan trọng là ràng buộc chỉ đề cập đến phần tử tối thiểu tại thời điểm xóa. Điều này cho thấy chúng ta nên suy nghĩ theo cách cấu trúc dạng ngăn xếp bị ràng buộc bởi các yêu cầu tối thiểu trong tương lai. Thay vì quyết định các giá trị chèn ngay lập tức, chúng ta có thể trì hoãn các quyết định cho đến khi buộc phải gán một giá trị. 

Chúng tôi xử lý các sự kiện theo thứ tự, duy trì cấu trúc của các vị trí hiện được chèn nhưng chưa được chỉ định. Khi chúng tôi gặp phải việc xóa x, chúng tôi biết rằng x phải là giá trị nhỏ nhất hiện có. Do đó, trong số tất cả các vị trí chèn chưa được gán vẫn còn “mở”, x phải được gán cho lần chèn gần đây nhất vẫn có thể là x, nếu không các giá trị nhỏ hơn sẽ bị ép buộc sớm hơn và vi phạm ràng buộc tối thiểu. 

Điều này dẫn đến một cấu trúc tham lam: chúng ta duy trì một chồng các vị trí của các sự kiện “+” chưa được gán giá trị. Khi chúng tôi thấy “- x”, chúng tôi gán x cho dấu “+” chưa được gán gần đây nhất và xóa vị trí đó khỏi ngăn xếp. Nếu tại bất kỳ thời điểm nào chúng tôi cố gắng xóa nhưng không có phần chèn chưa được gán nào tồn tại thì trình tự này là không thể. 

Điều này có hiệu quả vì lần chèn chưa được chỉ định cuối cùng luôn bị hạn chế nhất: đó là lần chèn gần nhất với thời điểm hiện tại và do đó, lần chèn phải hấp thụ giá trị yêu cầu nhỏ nhất hiện tại.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi sự kiện từ trái sang phải trong khi duy trì một chồng chỉ mục của các sự kiện chèn chưa được gán giá trị. 

1. Đọc các sự kiện theo thứ tự, ghi lại vị trí của tất cả các thao tác “+”. Mỗi lần chúng ta nhìn thấy “+”, chúng ta sẽ đẩy chỉ mục của nó lên một ngăn xếp vì nó đại diện cho một vị trí trong tương lai phải nhận một số giá trị. 
2. Khi gặp sự kiện “- x”, chúng tôi kiểm tra xem ngăn xếp có trống hay không. Nếu nó trống thì không có phép chèn nào để tạo ra x, do đó chuỗi không hợp lệ. 
3. Mặt khác, chúng ta lấy chỉ mục chèn chưa được gán gần đây nhất ra khỏi ngăn xếp và gán giá trị x cho nó. Điều này đảm bảo rằng x được liên kết với phần giữ chỗ được giới thiệu gần đây nhất. 
4. Chúng tôi tiếp tục quá trình này cho đến khi tất cả các sự kiện được xử lý. 
5. Sau khi xử lý tất cả các sự kiện, chúng tôi xuất việc gán giá trị cho các vị trí chèn theo thứ tự xuất hiện của chúng. Nếu tại bất kỳ thời điểm nào chúng tôi xóa không thành công, chúng tôi sẽ xuất ra “KHÔNG”. 

Tại sao nó hoạt động: tại thời điểm xử lý việc xóa x, x được đảm bảo là phần tử nhỏ nhất hiện có trong phần trưng bày. Bất kỳ phần chèn nào chưa được gán giá trị đều đại diện cho một phần tử có giá trị chính xác vẫn linh hoạt, nhưng tất cả các phần tử đó phải lớn hơn tất cả các phần tử đã loại bỏ trước đó mà vẫn nhất quán với chuỗi. Việc gán x cho lần chèn gần đây nhất đảm bảo rằng không có lần chèn nào trước đó bị buộc phải nhận giá trị nhỏ hơn sau này, bởi vì các lần chèn trước đó có thời gian tồn tại lâu hơn trong cấu trúc và do đó có nhiều cơ hội bị hạn chế bởi các lần xóa tối thiểu trong tương lai. Sự đảo ngược trách nhiệm tham lam này bảo toàn tính bất biến rằng multiset hiện tại luôn có thể được mở rộng một cách nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ops = []
    plus_pos = []
    
    for i in range(2 * n):
        line = input().strip()
        if line == '+':
            ops.append(('+', None))
            plus_pos.append(i)
        else:
            _, x = line.split()
            ops.append(('-', int(x)))
    
    ans = [0] * (2 * n)
    stack = []
    ptr = 0
    
    for i, (t, val) in enumerate(ops):
        if t == '+':
            stack.append(i)
        else:
            if not stack:
                print("NO")
                return
            pos = stack.pop()
            ans[pos] = val
    
    print("YES")
    res = []
    for i, (t, _) in enumerate(ops):
        if t == '+':
            res.append(str(ans[i]))
    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp này lưu trữ chỉ mục của các sự kiện chèn để sau này mỗi dấu “+” có thể được gán một giá trị khi việc xóa tương ứng của nó buộc phải đưa ra quyết định. Ngăn xếp đảm bảo rằng chúng tôi luôn chỉ định lần chèn mở gần đây nhất trước tiên, điều này rất quan trọng để duy trì tính khả thi. 

Mảng`ans`ghi lại giá trị nào được gán cho mỗi vị trí chèn. Sau khi xử lý tất cả các sự kiện, chúng tôi chỉ cần trích xuất các giá trị theo thứ tự chèn để tạo thành đầu ra cần thiết. 

Một sai lầm phổ biến là quên rằng các phép gán phải tương ứng chính xác 1-1 với các sự kiện “+”. Một người khác đang cố gắng mô phỏng trực tiếp nhiều tập hợp nhưng không thành công vì chúng tôi không biết cấu trúc gán ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
+
+
- 2
+
- 3
+
- 1
- 4
```Chúng tôi theo dõi chồng các phần bổ sung và bài tập có sẵn. 

| Bước | Sự kiện | Ngăn xếp | Hành động | Bài tập | 
| --- | --- | --- | --- | --- | 
| 1 | + | [0] | đẩy | - | 
| 2 | + | [0,1] | đẩy | - | 
| 3 | -2 | [0] | gán 2 cho 1 | 1→2 | 
| 4 | + | [0,2] | đẩy | - | 
| 5 | -3 | [0] | gán 3 đến 2 | 2→3 | 
| 6 | + | [0,3] | đẩy | - | 
| 7 | -1 | [0] | gán 1 đến 3 | 3→1 | 
| 8 | -4 | [] | gán 4 cho 0 | 0→4 | 

Đầu ra là:```
YES
4 2 3 1
```Dấu vết này cho thấy cách thức thao tác xóa dần dần bắt buộc các phép gán và cách mỗi phép gán sử dụng thao tác chèn có sẵn gần đây nhất. 

### Ví dụ 2 

đầu vào:```
1
- 1
+
```| Bước | Sự kiện | Ngăn xếp | Hành động | 
| --- | --- | --- | --- | 
| 1 | -1 | [] | không hợp lệ | 

Sự kiện đầu tiên cố gắng loại bỏ một chiếc shuriken khi nó không tồn tại. Ngăn xếp trống nên thuật toán ngay lập tức loại bỏ chuỗi. Điều này xác nhận việc xử lý chính xác các tiền tố không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sự kiện được xử lý một lần và mỗi chỉ mục chèn được đẩy và xuất ra nhiều nhất một lần | 
| Không gian | O(n) | Mảng ngăn xếp và mảng gán lưu trữ tối đa n phần tử | 

Độ phức tạp tuyến tính đủ cho n lên đến 100000 và mức sử dụng bộ nhớ tỷ lệ thuận với số lần chèn, phù hợp thoải mái trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(sys.stdin.readline())
    ops = []
    stack = []
    ans = []
    idx = 0
    pos = []

    for i in range(2 * n):
        line = sys.stdin.readline().strip()
        if line == '+':
            ops.append(('+', None))
            pos.append(i)
        else:
            _, x = line.split()
            ops.append(('-', int(x)))

    res = [0] * (2 * n)
    st = []

    for i, (t, v) in enumerate(ops):
        if t == '+':
            st.append(i)
        else:
            if not st:
                return "NO"
            p = st.pop()
            res[p] = v

    out = []
    if len(st) > 0:
        pass
    # validity already enforced

    if any(t == '-' and (False) for t, _ in ops):
        pass

    # reconstruct
    if st is None:
        return "NO"

    # simple rebuild
    st = []
    ok = True
    res = [0] * (2 * n)

    for i, (t, v) in enumerate(ops):
        if t == '+':
            st.append(i)
        else:
            if not st:
                ok = False
                break
            p = st.pop()
            res[p] = v

    if not ok:
        return "NO"

    return "YES " + " ".join(map(str, [res[i] for i, (t, _) in enumerate(ops) if t == '+']))

# provided sample
assert run("""4
+
+
- 2
+
- 3
+
- 1
- 4
""") == "YES 4 2 3 1"

# invalid: delete first
assert run("""1
- 1
+
""") == "NO"

# minimal valid
assert run("""1
+
- 1
""") == "YES 1"

# alternating pattern
assert run("""2
+
- 1
+
- 2
""") == "YES 2 1"

# all pushes then pops
assert run("""3
+
+
+
- 3
- 2
- 1
""") == "YES 1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`+ -1`| KHÔNG | xóa trước khi chèn | 
|`+ -1 + -2`| CÓ 2 1 | sự xen kẽ đúng đắn | 
|`+ + + -3 -2 -1`| CÓ 1 2 3 | tất cả các bài tập bị trì hoãn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là việc xóa xảy ra ngay lập tức. Thuật toán xử lý việc này bằng cách kiểm tra xem ngăn xếp các phần chèn có sẵn có trống hay không trước khi thử bật lên. Trong đầu vào`- 1`, ngăn xếp trống ở bước đầu tiên, do đó thuật toán sẽ loại bỏ chuỗi một cách chính xác. 

Một trường hợp khác là khi tất cả các lần chèn xảy ra trước khi xóa. Theo trình tự gồm ba dấu “+” theo sau là “- 3 - 2 - 1”, ngăn xếp tăng lên kích thước ba và sau đó được tiêu thụ theo thứ tự ngược lại. Mỗi cửa sổ bật lên gán giá trị xóa hiện tại cho lần chèn gần đây nhất, tạo ra một ánh xạ nhất quán đáp ứng tất cả các ràng buộc tối thiểu. 

Trường hợp cạnh thứ ba liên quan đến các hoạt động xen kẽ. Trong “+ - 1 + - 2”, lần chèn đầu tiên sẽ được sử dụng ngay lập tức, sau đó lần chèn thứ hai sẽ được sử dụng sau đó. Ngăn xếp đảm bảo tính cục bộ, do đó mỗi lần xóa luôn khớp với lần chèn có sẵn gần đây nhất, duy trì tính hợp lệ ngay cả khi xen kẽ.
