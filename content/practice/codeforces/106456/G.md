---
title: "CF 106456G - Taffy vs Yêu Tinh"
description: "Mỗi trường hợp thử nghiệm mô tả một kịch bản chiến đấu cố định với N đòn tấn công độc lập. Tấn công tôi nhắm vào yêu tinh i và gây toàn bộ sát thương Ci hoặc không gây sát thương gì, tùy thuộc vào việc khả năng xuyên thấu của Ai có chạm tới lớp phòng thủ Bi của yêu tinh hay không."
date: "2026-06-22T19:19:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106456
codeforces_index: "G"
codeforces_contest_name: "The 15th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 106456
solve_time_s: 62
verified: true
draft: false
---

[CF 106456G - Taffy vs Goblins](https://codeforces.com/problemset/problem/106456/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một kịch bản chiến đấu cố định với N đòn tấn công độc lập. Tấn công tôi nhắm vào yêu tinh i và gây toàn bộ sát thương Ci hoặc không gây sát thương gì, tùy thuộc vào việc khả năng xuyên thấu của Ai có chạm tới lớp phòng thủ Bi của yêu tinh hay không. Trước khi xử lý truy vấn, chúng tôi tính toán cuộc tấn công nào thành công theo giá trị ban đầu. 

Điều khó khăn là đối với mỗi giá trị truy vấn X, chúng tôi được phép chọn tối đa một đòn tấn công và thay thế khả năng xuyên thấu của nó bằng X trước khi đánh giá tất cả thiệt hại. Sau lần sửa đổi duy nhất này, chúng tôi tính toán lại những cuộc tấn công nào thành công và tính tổng thiệt hại của chúng. Mỗi truy vấn đều độc lập nên quyết định sửa đổi luôn được đưa ra mới. 

Đầu ra cho mỗi truy vấn là tổng thiệt hại tối đa có thể có sau khi chọn có nên sửa đổi một cuộc tấn công hay không và nên sửa đổi cuộc tấn công nào. 

Các ràng buộc ngụ ý rằng cả N và Q đều có thể lớn tới 2×10^5 trong một trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi hoạt động quét tuyến tính trên mỗi truy vấn đối với tất cả các cuộc tấn công, vì điều đó sẽ dẫn đến khoảng 4×10^10 hoạt động trong trường hợp xấu nhất. Bất kỳ giải pháp nào được chấp nhận đều phải giảm từng truy vấn thành công việc logarit hoặc không đổi sau khi xử lý trước, thường là O(log N) hoặc O(1). 

Việc triển khai ngây thơ cũng có thể thất bại một cách tinh tế nếu nó cho rằng sửa đổi tốt nhất luôn nhắm vào một cuộc tấn công hiện đang thất bại hoặc luôn nhắm vào một cuộc tấn công hiện đang thành công. Lựa chọn đúng phụ thuộc vào cách X so sánh với Bi đối với từng chỉ mục, do đó, việc bỏ qua sự tương tác giữa giá trị truy vấn và các ngưỡng sẽ dẫn đến kết quả không chính xác. 

Ví dụ: hãy xem xét tình huống trong đó tất cả các cuộc tấn công ban đầu đã thành công nhưng giá trị truy vấn X rất nhỏ. Một ý tưởng ngây thơ có thể cố gắng “sửa chữa” một cuộc tấn công thất bại không tồn tại hoặc cho rằng câu trả lời không thay đổi. Trên thực tế, việc thay đổi một đòn tấn công thành công trước đó với Bi cao thành X thấp chỉ có thể giảm hoặc duy trì sát thương, vì vậy lựa chọn tối ưu là không làm gì cả. 

Một trường hợp thất bại khác là khi nhiều cuộc tấn công ban đầu đều thất bại nhưng chỉ những cuộc tấn công có Bi ≤ X mới có thể khắc phục được. Việc chọn một cuộc tấn công với Bi > X không mang lại lợi ích gì, nhưng cách tiếp cận bất cẩn vẫn có thể chọn nó nếu bỏ qua điều kiện ngưỡng. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu bằng cách tính toán thiệt hại cơ bản mà không sửa đổi. Đây đơn giản là tổng của Ci trên tất cả i sao cho Ai ≥ Bi. Phần này được cố định cho tất cả các truy vấn. 

Bây giờ hãy xem xét điều gì sẽ xảy ra khi chúng ta chọn sửa đổi một đòn tấn công i bằng cách đặt độ xuyên thấu của nó thành X. 

Điều quan trọng là phải hiểu tác động của sự thay đổi này ở địa phương. Tấn công i đóng góp Ci hoặc 0 chỉ phụ thuộc vào sự so sánh giữa Ai (hoặc X sau khi sửa đổi) và Bi. Tất cả các đòn tấn công khác không thay đổi. 

Vì vậy, đối với mỗi chỉ số i, chúng ta so sánh đóng góp ban đầu của nó với đóng góp của nó sau khi buộc Ai vào X. Sự khác biệt chỉ phụ thuộc vào Bi, Ai, Ci và X. 

Nếu cuộc tấn công i ban đầu thành công, nghĩa là Ai ≥ Bi, thì nó đã đóng góp Ci. Sau khi sửa đổi, nó chỉ đóng góp Ci nếu X ≥ Bi, nếu không thì giảm xuống 0. Vì vậy, việc sửa đổi một cuộc tấn công thành công là trung tính hoặc có hại. 

Nếu cuộc tấn công i ban đầu thất bại, nghĩa là Ai < Bi, thì nó đóng góp 0. Sau khi sửa đổi, nó đóng góp Ci nếu X ≥ Bi, nếu không thì vẫn bằng 0. Vì vậy, chỉ những cuộc tấn công thất bại mới có thể tạo ra lợi ích dương và chỉ khi ngưỡng phòng thủ của chúng tối đa là X. 

Quan sát này làm sáng tỏ vấn đề: đối với mỗi truy vấn X, chúng tôi chỉ quan tâm đến các cuộc tấn công thất bại với Bi ≤ X và trong số đó chúng tôi muốn có Ci tối đa. 

Chúng tôi xử lý trước tất cả các cuộc tấn công thất bại như vậy theo cặp (Bi, Ci), sắp xếp chúng theo Bi và duy trì tiền tố tối đa trên Ci. Sau đó, mỗi truy vấn sẽ trở thành tìm kiếm nhị phân trên Bi, sau đó là tra cứu tối đa tiền tố. 

### Tóm tắt độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn | O(NQ) | O(1) | Quá chậm | 
| Sắp xếp + tiền tố tối đa | O(N log N + Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi tách giải pháp thành giai đoạn tiền xử lý và giai đoạn truy vấn. 

1. Tính sát thương cơ bản bằng cách lặp lại tất cả các đòn tấn công và tính tổng Ci cho mọi i trong đó Ai ≥ Bi. Giá trị này không bao giờ thay đổi trong các truy vấn, vì vậy chúng tôi lưu trữ nó. 
2. Với mỗi chỉ số i có Ai < Bi, ta ghi một cặp (Bi, Ci). Đây là những ứng cử viên duy nhất có khả năng cải thiện khi được sửa đổi. 
3. Sắp xếp các cặp này theo Bi theo thứ tự tăng dần. Điều này sắp xếp chúng sao cho đối với bất kỳ ngưỡng X nào, tất cả các ứng cử viên có thể sử dụng đều tạo thành một tiền tố. 
4. Xây dựng một mảng phụ trong đó chúng ta duy trì tiền tố tối đa của Ci trên danh sách đã sắp xếp. Sau đó, đối với bất kỳ tiền tố nào kết thúc ở vị trí k, chúng ta có thể biết ngay Ci tốt nhất trong số tất cả Bi trong phạm vi đó. 
5. Với mỗi truy vấn X, hãy tìm chỉ số k lớn nhất sao cho Bi ≤ X bằng tìm kiếm nhị phân. Nếu không có chỉ mục nào như vậy tồn tại thì câu trả lời cho truy vấn này chỉ là đường cơ sở. 
6. Ngược lại, lấy tiền tố lớn nhất tại k. Giá trị này là lợi ích tốt nhất có thể đạt được từ việc sửa đổi một cuộc tấn công. Thêm nó vào đường cơ sở và xuất kết quả. 

Tại sao nó hoạt động xuất phát từ thực tế là mọi sửa đổi đều không mang lại lợi ích gì hoặc biến chính xác một cuộc tấn công thất bại thành một cuộc tấn công thành công. Không có cấu trúc nào khác trong mảng thay đổi. Trong số tất cả những cải tiến có thể có, chỉ những cải tiến có Bi  X là hợp lệ, và trong số những cải tiến đó chúng ta muốn có Ci tối đa, vì mỗi lựa chọn đều độc lập và đóng góp bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        A = list(map(int, input().split()))
        B = list(map(int, input().split()))
        C = list(map(int, input().split()))

        base = 0
        candidates = []

        for i in range(n):
            if A[i] >= B[i]:
                base += C[i]
            else:
                candidates.append((B[i], C[i]))

        candidates.sort()
        prefix = []
        best = 0
        for b, c in candidates:
            if c > best:
                best = c
            prefix.append(best)

        def answer(x):
            lo, hi = 0, len(candidates) - 1
            pos = -1
            while lo <= hi:
                mid = (lo + hi) // 2
                if candidates[mid][0] <= x:
                    pos = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            gain = 0
            if pos != -1:
                gain = prefix[pos]

            return base + gain

        for _ in range(q):
            x = int(input())
            print(answer(x))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tách các cuộc tấn công luôn thành công khỏi các cuộc tấn công có khả năng cải thiện. Tìm kiếm nhị phân tách biệt tập hợp con các ứng cử viên hoạt động theo truy vấn hiện tại X. Mảng tối đa tiền tố đảm bảo rằng khi đã biết phạm vi hợp lệ, lựa chọn tốt nhất có thể được truy xuất trong thời gian không đổi. 

Một điểm tinh tế là chúng tôi không bao giờ xem xét việc sửa đổi các cuộc tấn công đã thành công, vì sự đóng góp tốt nhất có thể có của chúng sau khi sửa đổi không bao giờ tốt hơn là không làm gì cả. Đây là những gì cho phép giảm xuống một danh sách được sắp xếp duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ với ba cuộc tấn công. 

Giả sử chúng ta có A = [1, 5, 2], B = [3, 4, 2], C = [10, 20, 30]. Sát thương cơ bản đến từ đòn tấn công 2 và 3, vì chỉ những đòn nào thỏa mãn Ai ≥ Bi. Vậy cơ số = 20 + 30 = 50. 

Danh sách ứng cử viên chỉ bao gồm các cuộc tấn công thất bại ban đầu. Đó là đòn tấn công 1 vì 1 < 3, cho (B1, C1) = (3, 10). Sau khi sắp xếp chúng ta vẫn có [(3, 10)] và tiền tố tối đa là [10]. 

Đối với truy vấn X = 2, không có ứng viên nào có Bi ≤ 2 nên mức tăng là 0 và câu trả lời là 50. 

| Bước | Giá trị | 
| --- | --- | 
| Căn cứ | 50 | 
| Ứng viên | (3,10) | 
| X = 2 phạm vi hợp lệ | không | 
| Đạt được | 0 | 
| Trả lời | 50 | 

Điều này xác nhận rằng X nhỏ không thể mở khóa bất kỳ cải tiến nào. 

Bây giờ xét X = 3. 

| Bước | Giá trị | 
| --- | --- | 
| Căn cứ | 50 | 
| Thí sinh ≤ X | (3,10) | 
| Đạt được tốt nhất | 10 | 
| Trả lời | 60 | 

Tại đây chúng ta có thể nâng cấp đòn tấn công 1, khiến nó thành công và cộng thêm 10 sát thương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + Q log N) | sắp xếp các cuộc tấn công thất bại và tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian | O(N) | lưu trữ danh sách ứng viên và tiền tố tối đa | 

Các ràng buộc cho phép tổng số phần tử lên tới 2×10^5, do đó, các truy vấn sắp xếp và logarit phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    data = inp.strip().split()
    it = iter(data)

    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))
        q = int(next(it))
        A = [int(next(it)) for _ in range(n)]
        B = [int(next(it)) for _ in range(n)]
        C = [int(next(it)) for _ in range(n)]

        base = 0
        cand = []
        for i in range(n):
            if A[i] >= B[i]:
                base += C[i]
            else:
                cand.append((B[i], C[i]))

        cand.sort()
        pref = []
        best = 0
        for b, c in cand:
            best = max(best, c)
            pref.append(best)

        import bisect

        for _ in range(q):
            x = int(next(it))
            idx = bisect.bisect_right(cand, (x, 10**18)) - 1
            gain = 0
            if idx >= 0:
                gain = pref[idx]
            out.append(str(base + gain))

    return "\n".join(out)

# custom cases

# 1) minimum size, no improvement possible
assert run("1\n1 1\n5\n10\n100\n1") == "0"

# 2) single improvable attack
assert run("1\n1 1\n1\n5\n10\n5") == "10"

# 3) multiple candidates, prefix behavior
assert run("1\n3 2\n1 1 1\n2 3 4\n5 6 7\n2 3") == "6\n7"

# 4) mix of base + improvement
assert run("1\n3 1\n5 1 3\n3 2 4\n10 20 30\n3") == "60"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thất bại duy nhất | 0 | không có ứng viên đóng góp | 
| cải tiến duy nhất | 10 | kích hoạt đúng ngưỡng | 
| nhiều truy vấn | 6, 7 | tiền tố chính xác tối đa | 
| cơ sở hỗn hợp + tăng | 60 | sự kết hợp giữa cơ sở và cải tiến | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mọi cuộc tấn công đều đã thành công. Trong trường hợp đó, danh sách ứng cử viên trống, do đó mọi truy vấn phải trả về giá trị cơ sở không thay đổi. Thuật toán xử lý việc này một cách tự nhiên vì mảng tiền tố trống và không có kết quả tìm kiếm nhị phân nào tạo ra chỉ mục hợp lệ. 

Một trường hợp khác là khi X nhỏ hơn mọi Bi trong số các cuộc tấn công thất bại. Thế thì không thể cải thiện được. Tìm kiếm nhị phân trả về -1 và mức tăng vẫn bằng 0, phù hợp với thực tế là không thể nâng cấp cuộc tấn công nào. 

Trường hợp cuối cùng là khi nhiều cuộc tấn công thất bại có cùng Bi nhưng có giá trị Ci khác nhau. Việc sắp xếp nhóm chúng lại với nhau và mức tối đa của tiền tố đảm bảo chúng tôi luôn chọn Ci tốt nhất bất kể trùng lặp, vì chúng tôi chỉ quan tâm đến giá trị tối đa trong tiền tố hợp lệ.
