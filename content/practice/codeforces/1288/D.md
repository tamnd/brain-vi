---
problem: 1288D
contest_id: 1288
problem_index: D
name: "Minimax Problem"
contest_name: "Educational Codeforces Round 80 (Rated for Div. 2)"
rating: 2000
tags: ["binary search", "bitmasks", "dp"]
answer: passed_samples
verified: true
solve_time_s: 161
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2db7e1-74b8-83ec-b93f-88d597a14c54
---

#CF 1288D - Vấn đề Minimax 

**Xếp hạng:** 2000 
**Thẻ:** tìm kiếm nhị phân, mặt nạ bit, dp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2db7e1-74b8-83ec-b93f-88d597a14c54 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta có một tập hợp n vectơ, mỗi vectơ có độ dài m. Hãy coi mỗi vectơ như mô tả m thuộc tính của một đối tượng. Chúng ta được phép chọn hai đồ vật, có thể giống nhau hai lần. Sau khi chọn chúng, chúng tôi sẽ xây dựng một vectơ mới bằng cách lấy giá trị tối đa theo tọa độ của các thuộc tính của chúng. Từ vectơ kết quả này, chúng tôi chỉ quan tâm đến tọa độ yếu nhất của nó, nghĩa là giá trị tối thiểu trong số m vị trí của nó. 

Mục tiêu là chọn hai vectơ gốc sao cho tọa độ yếu nhất này sau khi hợp nhất càng lớn càng tốt. Nói cách khác, chúng ta muốn chọn hai hàng sao cho ngay cả tọa độ nhỏ nhất trong tọa độ tối đa của chúng cũng lớn nhất có thể. 

Cấu trúc của bài toán trở nên có ý nghĩa khi ta nhận thấy m rất nhỏ, nhiều nhất là 8, trong khi n có thể rất lớn, lên tới 300.000. Sự bất đối xứng này là tín hiệu chính: chúng tôi dự kiến ​​sẽ nén hoặc mã hóa từng mảng bằng cách sử dụng mặt nạ bit hoặc lý luận tập hợp con theo tọa độ, thay vì so sánh tất cả các cặp. 

Một cách tiếp cận đơn giản sẽ thử tất cả các cặp mảng, tính toán mức tối đa theo tọa độ của chúng, sau đó lấy mức tối thiểu, cho O (n^2 m), quá chậm khi n lớn. 

Chế độ thất bại tinh vi hơn xuất hiện khi thử tối đa hóa tham lam trên mỗi tọa độ một cách độc lập. Ví dụ: chọn một mảng tốt nhất ở tọa độ này và mảng khác tốt nhất ở tọa độ khác vẫn có thể tạo ra mức tối thiểu kém sau khi hợp nhất, vì tọa độ tương tác thông qua thao tác tối thiểu. 

Khó khăn thực sự là sự thống trị không phải là sự thống trị độc lập trên mỗi tọa độ; chúng ta cần một cặp bao phủ tất cả các tọa độ trên một ngưỡng nào đó. 

## Phương pháp tiếp cận 

Nếu chúng tôi thử tất cả các cặp (i, j), chúng tôi sẽ tính toán mảng đã hợp nhất và đánh giá mức tối thiểu của nó. Điều này đúng nhưng yêu cầu khoảng n2/2 phép so sánh và mỗi phép so sánh tốn m phép tính. Với n = 300.000 thì điều này là không thể. 

Quan sát chính là đảo ngược quan điểm. Thay vì tối đa hóa trực tiếp giá trị tối thiểu của mảng đã hợp nhất, chúng tôi đoán một câu trả lời ứng viên là x và hỏi liệu có tồn tại một cặp mảng có tọa độ tối đa ít nhất là x trong mọi tọa độ hay không. 

Việc cố định ngưỡng x sẽ biến mỗi mảng thành một mặt nạ bit có độ dài m, trong đó bit k là 1 nếu a_{i,k} ≥ x. Bây giờ câu hỏi trở thành: liệu chúng ta có thể chọn hai mặt nạ bit có bitwise OR bằng tất cả các bit không? 

Nếu chúng ta có thể giải quyết việc kiểm tra sự tồn tại này cho một x cho trước thì chúng ta có thể tìm kiếm nhị phân x trên phạm vi giá trị trong mảng. 

Cải tiến tinh tế so với tìm kiếm nhị phân đơn giản xuất phát từ thực tế là m ≤ 8. Điều này cho phép chúng ta biểu diễn mỗi mặt nạ dưới dạng một số nguyên và tính toán trước có bao nhiêu mảng tương ứng với mỗi mặt nạ. Sau đó, đối với mỗi mặt nạ, chúng tôi có thể kiểm tra xem có mặt nạ đối tác nào có thể che phủ toàn bộ mặt nạ đó hay không. 

Thách thức là chúng ta cũng cần trả về các chỉ số của mảng thực tế chứ không chỉ tính khả thi. Vì vậy, chúng tôi lưu trữ một chỉ mục đại diện cho mỗi mặt nạ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² m) | O(1) | Quá chậm | 
| Mặt nạ + Tìm kiếm nhị phân | O(n m log V + 2^m m) | O(2^m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chúng tôi tìm kiếm nhị phân giá trị câu trả lời x trong phạm vi các giá trị có thể có trong đầu vào. Điều này có hiệu quả vì nếu có thể đạt được ngưỡng x thì bất kỳ ngưỡng nào nhỏ hơn cũng có thể đạt được, tạo ra một vị từ đơn điệu. 
2. Đối với x cố định, chúng tôi chuyển đổi mọi mảng thành mặt nạ bit có kích thước m, trong đó bit thứ k được đặt nếu giá trị thứ k ít nhất là x. Điều này nén từng mảng thành một cấu trúc nắm bắt chính xác tọa độ mà nó có thể đáp ứng ở ngưỡng x. 
3. Chúng tôi nhóm các mảng theo mặt nạ của chúng và lưu trữ ít nhất một chỉ mục cho mỗi mặt nạ. Điều này là đủ vì chúng ta chỉ cần tìm một cặp hợp lệ chứ không phải liệt kê tất cả các cặp. 
4. Chúng tôi kiểm tra tất cả các cặp mặt nạ (hoặc hiệu quả hơn, đối với mỗi mặt nạ, chúng tôi cố gắng tìm mặt nạ bổ sung) sao cho OR theo bit của hai mặt nạ là (1 << m) - 1. Điều kiện này đảm bảo rằng mọi tọa độ đều được bao phủ bởi ít nhất một trong hai mảng ở ngưỡng x. 
5. Nếu một cặp như vậy tồn tại, chúng tôi lưu trữ các chỉ số tương ứng và đánh dấu x là khả thi; nếu không thì x quá lớn. 
6. Sau khi tìm kiếm nhị phân kết thúc, chúng tôi xuất cặp được lưu trữ tương ứng với x lớn nhất khả thi. 

Bước suy luận quan trọng là tính khả thi chỉ phụ thuộc vào việc tọa độ có được bao phủ độc lập bởi ít nhất một trong hai mảng hay không. Việc giảm bitmask nắm bắt chính xác điều này. 

### Tại sao nó hoạt động 

Đối với bất kỳ ngưỡng x cố định nào, một mảng sẽ đóng góp 1 trong tọa độ k nếu nó đủ để bao phủ tọa độ đó ở cấp x. Mảng được hợp nhất sử dụng max tương ứng chính xác với bit OR của các mặt nạ này. Giá trị tối thiểu của mảng được hợp nhất ít nhất là x tương đương với mọi bit được đặt trong kết quả OR. Do đó, việc giải quyết vấn đề giảm xuống còn việc tìm hai mặt nạ có OR bao phủ toàn bộ và tìm kiếm nhị phân trên x duy trì tính chính xác vì tính khả thi là đơn điệu trong x. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(x, a, n, m):
    full = (1 << m) - 1
    cnt = {}
    first_idx = {}

    for i in range(n):
        mask = 0
        row = a[i]
        for j in range(m):
            if row[j] >= x:
                mask |= (1 << j)
        if mask not in first_idx:
            first_idx[mask] = i
        cnt[mask] = cnt.get(mask, 0) + 1

    masks = list(cnt.keys())

    for m1 in masks:
        for m2 in masks:
            if (m1 | m2) == full:
                return first_idx[m1], first_idx[m2]

    return None

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    vals = set()
    for row in a:
        vals.update(row)
    vals = sorted(vals)

    lo, hi = 0, len(vals) - 1
    best_pair = (0, 0)

    def feasible(x):
        res = check(vals[x], a, n, m)
        if res is None:
            return False, None
        return True, res

    l, r = 0, len(vals) - 1
    while l <= r:
        mid = (l + r) // 2
        ok, res = feasible(mid)
        if ok:
            best_pair = res
            l = mid + 1
        else:
            r = mid - 1

    print(best_pair[0] + 1, best_pair[1] + 1)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén từng mảng thành một mặt nạ bit tùy thuộc vào ngưỡng. Chức năng kiểm tra xây dựng bản đồ tần số mặt nạ và lưu trữ một chỉ mục đại diện cho mỗi mặt nạ. Sau đó, nó bắt buộc tất cả các cặp mặt nạ, điều này khả thi vì có tối đa 2^m = 256 mặt nạ. 

Tìm kiếm nhị phân được thực hiện trên các giá trị duy nhất được sắp xếp từ đầu vào, vì câu trả lời phải trùng với một số giá trị tọa độ. Điều này tránh việc tìm kiếm giá trị thả nổi hoặc tùy ý và giữ cho việc kiểm tra tính khả thi được riêng biệt. 

Một điểm tinh tế phổ biến là chỉ lưu trữ một chỉ mục cho mỗi mặt nạ. Điều này hợp lệ vì bất kỳ cặp mặt nạ hợp lệ nào cũng đủ và chúng tôi không cần nhiều đại diện. 

## Ví dụ đã hoạt động 

Chúng tôi minh họa việc kiểm tra tính khả thi trên một kịch bản đơn giản hóa với m = 3. 

### Ví dụ 1 

Mảng đầu vào:```
[5 0 3]
[1 8 9]
[9 1 0]
```Giả sử ngưỡng x = 5. 

| Mảng | Mặt nạ | 
| --- | --- | 
| [5 0 3] | 100 | 
| [1 8 9] | 011 | 
| [9 1 0] | 100 | 

Chúng tôi kiểm tra các cặp: 

| m1 | m2 | HOẶC | Đầy? | 
| --- | --- | --- | --- | 
| 100 | 011 | 111 | vâng | 

Vậy cặp này hợp lệ. Điều này xác nhận rằng việc hợp nhất mảng 1 và 2 bao gồm tất cả các tọa độ ở ngưỡng 5. 

### Ví dụ 2 

Mảng giống nhau, ngưỡng x = 9. 

| Mảng | Mặt nạ | 
| --- | --- | 
| [5 0 3] | 000 | 
| [1 8 9] | 001 | 
| [9 1 0] | 100 | 

Không có cặp mặt nạ nào có thể tạo ra 111, vì không có mảng nào bao trùm tất cả tọa độ tại 9. Điều này khẳng định tính không khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m + 2^{2m} · log n) | Mỗi lần kiểm tra tính khả thi sẽ xây dựng các mặt nạ theo O(nm) và việc ghép nối mặt nạ được giới hạn bởi 256² vì m ≤ 8 | 
| Không gian | O(2^m) | Kho chứa khẩu trang và người đại diện | 

Các ràng buộc cho phép điều này một cách thoải mái vì 2^8 = 256, làm cho không gian mặt nạ không đổi một cách hiệu quả và n·m ≤ 2,4 triệu thao tác trên mỗi lần kiểm tra có thể chấp nhận được bằng cách cắt bớt và triển khai chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n, m = map(int, sys.stdin.readline().split())
    a = [list(map(int, sys.stdin.readline().split())) for _ in range(n)]

    vals = sorted({v for row in a for v in row})

    def check(x):
        full = (1 << m) - 1
        first = {}
        masks = set()

        for i in range(n):
            mask = 0
            for j in range(m):
                if a[i][j] >= x:
                    mask |= (1 << j)
            masks.add(mask)
            if mask not in first:
                first[mask] = i

        masks = list(masks)
        for m1 in masks:
            for m2 in masks:
                if (m1 | m2) == full:
                    return first[m1], first[m2]
        return None

    l, r = 0, len(vals) - 1
    ans = (0, 0)
    while l <= r:
        mid = (l + r) // 2
        res = check(vals[mid])
        if res:
            ans = res
            l = mid + 1
        else:
            r = mid - 1

    return f"{ans[0]+1} {ans[1]+1}"

# sample
assert run("""6 5
5 0 3 1 2
1 8 9 1 3
1 2 3 4 5
9 1 0 3 7
2 3 0 6 3
6 4 1 7 0
""") == "1 5"

# minimum case
assert run("""1 1
5
""") == "1 1"

# all equal
assert run("""3 3
1 1 1
1 1 1
1 1 1
""") == "1 1"

# distinct dominance
assert run("""3 2
1 2
2 1
3 0
""") in ["1 2", "2 1"]

# boundary
assert run("""2 2
0 0
100 0
""") == "1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 1 | trường hợp nhận dạng tầm thường | 
| tất cả các mảng bằng nhau | 1 1 | tự ghép nối đúng | 
| sự thống trị bất đối xứng | 1 2 hoặc 2 1 | đối xứng chọn cặp | 
| số không ranh giới | 1 2 | xử lý tọa độ thấp | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi một mảng đã thỏa mãn tất cả các tọa độ trên ngưỡng cao. Trong trường hợp đó, việc ghép nối nó với chính nó vẫn được coi là hợp lệ. Biểu diễn mặt nạ bit xử lý việc này vì bản thân mặt nạ đầy đủ HOẶC vẫn đầy, do đó thuật toán tự nhiên trả về (i, i). 

Một trường hợp tinh tế khác là khi nhiều mảng chia sẻ các mặt nạ giống hệt nhau. Nếu chúng ta chỉ lưu trữ số lượng mà không lưu trữ chỉ mục, chúng ta sẽ mất khả năng xây dựng lại một cặp hợp lệ. Việc lưu trữ một chỉ mục đại diện cho mỗi mặt nạ đảm bảo luôn có thể tái thiết mà không ảnh hưởng đến tính chính xác. 

Trường hợp thứ ba là khi giải pháp tối ưu sử dụng hai mảng giống hệt nhau và không có sự phân bổ giá trị duy nhất. Điều kiện mặt nạ OR vẫn cho phép m1 = m2, do đó thuật toán xem xét chính xác các cặp tự thay vì buộc các chỉ số riêng biệt.
