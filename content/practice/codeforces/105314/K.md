---
title: "CF 105314K - Hội chứng Ahmad và khác biệt"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng ta được xem một hàng thí sinh, mỗi người mang một quả bóng có màu nào đó được biểu thị bằng một số nguyên."
date: "2026-06-23T15:04:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105314
codeforces_index: "K"
codeforces_contest_name: "Robbing Balloons 2.0 Qualifications"
rating: 0
weight: 105314
solve_time_s: 50
verified: true
draft: false
---

[CF 105314K - Hội chứng Ahmad và Đặc biệt](https://codeforces.com/problemset/problem/105314/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng ta được xem một hàng thí sinh, mỗi người mang một quả bóng có màu nào đó được biểu thị bằng một số nguyên. Nhiệm vụ là tìm một đoạn liền kề của dòng này vẫn chứa mọi màu riêng biệt xuất hiện ở bất kỳ đâu trong toàn bộ mảng và trong số tất cả các đoạn như vậy, chúng ta muốn có đoạn ngắn nhất có thể. 

Nói cách khác, trước tiên chúng ta ngầm xác định tập hợp tất cả các giá trị duy nhất trong mảng. Sau đó, chúng tôi tìm kiếm mảng con nhỏ nhất có tập hợp các giá trị khớp chính xác với tập hợp toàn cục này. 

Ràng buộc chính là tổng số phần tử trong tất cả các trường hợp thử nghiệm có thể đạt tới 10^6. Điều này ngay lập tức loại trừ mọi giải pháp kiểm tra tất cả các mảng con một cách rõ ràng. Một phép liệt kê O(n^2) đơn giản cho tất cả các mảng con sẽ yêu cầu khoảng 10^12 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi trong 2,5 giây. Ngay cả các cách tiếp cận O(n^2 log n) cũng vô vọng. 

Vấn đề về cơ bản là yêu cầu cửa sổ tối thiểu theo trình tự bao gồm tất cả các phần tử riêng biệt. Khó khăn không phải là xác định tập hợp màu mà là nén tập hợp đó thành phạm vi liền kề nhỏ nhất. 

Một số trường hợp đặc biệt nêu bật những cạm bẫy của lý luận ngây thơ. Nếu tất cả các phần tử đều giống hệt nhau, ví dụ [5, 5, 5, 5] thì câu trả lời là 1 vì bất kỳ phần tử đơn lẻ nào cũng đã bao gồm tất cả các màu riêng biệt. Một cách tiếp cận ngây thơ mà quên tính toán trước số lượng riêng biệt có thể vẫn hoạt động, nhưng nhiều cách triển khai không chính xác vô tình quét ra ngoài từ mọi vị trí và đếm quá mức. 

Một trường hợp tinh tế khác là khi mảng đã ở mức tối thiểu ở giữa, chẳng hạn như [1, 2, 3, 2, 1]. Phân đoạn tối ưu là toàn bộ mảng vì tất cả các màu đều cần thiết, nhưng trong [1, 2, 1, 3, 2], phân đoạn tối thiểu cũng là toàn bộ mảng mặc dù vẫn tồn tại sự lặp lại. Điều này thường phá vỡ những nỗ lực tham lam cố gắng thu mình lại từ một phía. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xem xét mọi mảng con có thể có, tính toán tập hợp các phần tử bên trong nó và so sánh nó với tập hợp toàn cục của tất cả các phần tử. Đối với mỗi mảng con O(n^2), việc xây dựng tập hợp có giá O(n) trong trường hợp xấu nhất nếu được thực hiện một cách ngây thơ hoặc ít nhất O(1) được khấu hao khi bảo trì cẩn thận nhưng vẫn cập nhật tổng số O(n^2). Điều này dẫn đến hành vi O(n^3) hoặc O(n^2) tùy thuộc vào kiểu triển khai, quá chậm đối với n lên tới 2 × 10^5. 

Quan sát quan trọng là chúng ta không cần các mảng con tùy ý. Chúng tôi chỉ quan tâm đến các cửa sổ chứa tất cả các giá trị riêng biệt. Khi chúng ta biết có bao nhiêu giá trị riêng biệt tồn tại trên toàn cầu, bài toán sẽ trở thành bài toán cổ điển “cửa sổ tối thiểu chứa tất cả các phần tử bắt buộc”. Thay vì tính toán lại các bộ từ đầu, chúng tôi duy trì một cửa sổ trượt với số lượng tần số. 

Chúng ta mở rộng con trỏ bên phải để bao gồm các phần tử cho đến khi cửa sổ chứa tất cả các màu riêng biệt. Sau đó, chúng tôi thu nhỏ con trỏ bên trái nhiều nhất có thể trong khi vẫn duy trì phạm vi bao phủ. Mỗi khi cửa sổ hợp lệ, chúng tôi sẽ cập nhật câu trả lời. Mỗi con trỏ di chuyển nhiều nhất là n lần, do đó độ phức tạp tổng cộng sẽ trở thành tuyến tính cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) hoặc O(n^2 · khác biệt) | O(n) | Quá chậm | 
| Cửa Sổ Trượt Tối Ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập bằng cách sử dụng cửa sổ trượt hai con trỏ trên mảng.

1. Đầu tiên hãy tính tổng số màu riêng biệt trong mảng. Điều này được thực hiện bằng cách chèn tất cả các giá trị vào một tập hợp hoặc bản đồ tần số. Giá trị này thể hiện chính xác số phần tử duy nhất mà cửa sổ của chúng ta phải chứa để hợp lệ. 
2. Khởi tạo hai con trỏ, trái và phải, cả hai đều bắt đầu từ vị trí 0. Chúng tôi cũng duy trì bản đồ tần số cho các phần tử bên trong cửa sổ hiện tại và bộ đếm theo dõi số lượng màu riêng biệt hiện được đáp ứng trong cửa sổ. 
3. Mở rộng con trỏ bên phải từng bước. Mỗi lần chúng tôi thêm một phần tử mới, chúng tôi sẽ cập nhật tần số của nó. Nếu phần tử này xuất hiện lần đầu tiên trong cửa sổ, chúng tôi sẽ tăng số lượng màu riêng biệt được che phủ. Chúng tôi tiếp tục mở rộng cho đến khi cửa sổ chứa tất cả các màu riêng biệt cần thiết. 
4. Khi cửa sổ trở nên hợp lệ, chúng tôi sẽ cố gắng thu nhỏ cửa sổ từ bên trái. Chúng tôi di chuyển con trỏ trái về phía trước trong khi vẫn giữ tất cả các màu riêng biệt. Mỗi lần chúng tôi xóa một phần tử, chúng tôi sẽ cập nhật tần số của nó và nếu tần số giảm xuống 0, chúng tôi sẽ giảm số lượng riêng biệt được đề cập. 
5. Sau mỗi thao tác thu nhỏ thành công khi cửa sổ vẫn hợp lệ, chúng tôi cập nhật câu trả lời với độ dài cửa sổ hiện tại. 
6. Tiếp tục quá trình này cho đến khi con trỏ bên phải đến cuối mảng. 

Ý tưởng quan trọng là mỗi khi chúng tôi sửa ranh giới bên phải, chúng tôi sẽ đẩy ranh giới bên trái càng xa càng tốt mà không làm mất tính hợp lệ, đảm bảo rằng mọi cửa sổ hợp lệ tối thiểu đều được xem xét chính xác một lần. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì một cửa sổ phản ánh chính xác sự phân bố tần số của các phần tử giữa trái và phải. Điều bất biến là bất cứ khi nào cửa sổ được đánh dấu hợp lệ, nó sẽ chứa tất cả các phần tử riêng biệt của toàn bộ mảng. Bởi vì chúng tôi chỉ thu nhỏ khi tính hợp lệ được bảo toàn nên chúng tôi không bao giờ bỏ qua khoảng thời gian tối thiểu ứng cử viên. Vì mỗi khi di chuyển sang trái, chúng tôi chỉ loại bỏ các phần tử trùng lặp không cần thiết, nên mọi giải pháp tối ưu đều phải phù hợp với một số điểm cuối bên phải và chúng tôi liệt kê tất cả các điểm cuối đó một cách có hệ thống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        total_distinct = len(set(a))

        freq = {}
        have = 0
        left = 0
        ans = n

        for right in range(n):
            x = a[right]
            freq[x] = freq.get(x, 0) + 1
            if freq[x] == 1:
                have += 1

            while have == total_distinct:
                ans = min(ans, right - left + 1)
                y = a[left]
                freq[y] -= 1
                if freq[y] == 0:
                    have -= 1
                left += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán có bao nhiêu màu riêng biệt tồn tại trên toàn cầu bằng cách sử dụng một tập hợp xác định điều kiện mục tiêu cho bất kỳ cửa sổ hợp lệ nào. Từ điển tần số theo dõi số lần mỗi màu xuất hiện bên trong cửa sổ hiện tại và biến này theo dõi số lượng màu riêng biệt hiện được thể hiện ít nhất một lần. 

Con trỏ bên phải sẽ mở rộng cửa sổ và mỗi phần tử mới sẽ giới thiệu một màu riêng biệt mới hoặc tăng tần số hiện có. Khi có tổng_distinct bằng, cửa sổ sẽ hợp lệ và chúng tôi cố gắng thu gọn nó từ bên trái trong khi vẫn duy trì tính hợp lệ. Câu trả lời được cập nhật ở mỗi bước rút gọn hợp lệ, đảm bảo chúng tôi nắm bắt được cửa sổ nhỏ nhất kết thúc ở mỗi ranh giới bên phải. 

Một lỗi tinh vi phổ biến là chỉ cập nhật câu trả lời sau giai đoạn mở rộng đầy đủ. Điều đó bỏ lỡ các cửa sổ hợp lệ ngắn hơn được tạo trong quá trình co lại. Một lỗi khác là không giảm được khi tần số đạt đến 0, điều này cho rằng một màu vẫn còn tồn tại một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [2, 2, 1, 3]
```Các màu riêng biệt là {1, 2, 3}, vì vậy mục tiêu là 3. 

| đúng | trái | cửa sổ | trạng thái tần số | có | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [2] | {2:1} | 1 | mở rộng | 
| 1 | 0 | [2,2] | {2:2} | 1 | mở rộng | 
| 2 | 0 | [2,2,1] | {2:2,1:1} | 2 | mở rộng | 
| 3 | 0 | [2,2,1,3] | {2:2,1:1,3:1} | 3 | hợp lệ, thu nhỏ | 

Bây giờ thu nhỏ lại: 

| đúng | trái | cửa sổ | trạng thái tần số | có | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | [2,1,3] | {2:1,1:1,3:1} | 3 | 
| 3 | 2 | [1,3] | {1:1,3:1} | 2 điểm dừng | 

Câu trả lời hay nhất là 3. 

Điều này cho thấy rằng các bản sao của 2 bên trái sẽ không liên quan khi có tất cả các màu. 

### Ví dụ 2 

đầu vào:```
a = [1, 2, 1, 3, 2]
```Các màu riêng biệt là {1, 2, 3}. 

| đúng | trái | cửa sổ | có | 
| --- | --- | --- | --- | 
| 0 | 0 | [1] | 1 | 
| 1 | 0 | [1,2] | 2 | 
| 2 | 0 | [1,2,1] | 2 | 
| 3 | 0 | [1,2,1,3] | 3 hợp lệ | 
| 3 | 1 | [2,1,3] | 3 | 
| 3 | 2 | [1,3] | 2 điểm dừng | 
| 4 | 2 | [1,3,2] | 3 hợp lệ | 
| 4 | 3 | [3,2] | 2 điểm dừng | 

Độ dài cửa sổ tối thiểu là 3. 

Điều này chứng tỏ rằng tồn tại nhiều cửa sổ hợp lệ và thuật toán nắm bắt chính xác tất cả chúng bằng cách neo ở mỗi điểm cuối bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi con trỏ di chuyển tối đa n bước và các cập nhật từ điển được khấu hao O(1) | 
| Không gian | O(n) | Bản đồ tần số có thể lưu trữ tất cả các phần tử riêng biệt | 

Tổng kích thước đầu vào lên tới 10^6 và quét tuyến tính cho mỗi trường hợp thử nghiệm giúp duy trì thời gian chạy tốt trong giới hạn. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ tần số, tuyến tính về số lượng các giá trị riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        total_distinct = len(set(a))
        freq = {}
        have = 0
        left = 0
        ans = n

        for right in range(n):
            x = a[right]
            freq[x] = freq.get(x, 0) + 1
            if freq[x] == 1:
                have += 1

            while have == total_distinct:
                ans = min(ans, right - left + 1)
                y = a[left]
                freq[y] -= 1
                if freq[y] == 0:
                    have -= 1
                left += 1

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided sample
assert run("1\n4\n2 2 1 3\n") == "3\n"

# all equal
assert run("1\n5\n7 7 7 7 7\n") == "1\n"

# already minimal spread
assert run("1\n3\n1 2 3\n") == "3\n"

# duplicates with shrink possibility
assert run("1\n5\n1 2 1 3 2\n") == "3\n"

# large repeated pattern
assert run("1\n6\n1 1 2 2 3 3\n") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 | trường hợp phần tử riêng biệt duy nhất | 
| 1 2 3 | 3 | bảo hiểm đầy đủ mà không trùng lặp | 
| trùng lặp hỗn hợp | 3 | trượt co chính xác | 
| cặp lặp lại | 3 | nhiều cửa sổ hợp lệ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các phần tử đều giống hệt nhau, ví dụ [4, 4, 4, 4]. Thuật toán tính Total_distinct là 1. Lần đầu tiên bên phải chuyển sang chỉ mục 0, cửa sổ đã hợp lệ. Bước thu nhỏ ngay lập tức tăng sang trái cho đến khi chuyển sang phải, nhưng cửa sổ được ghi tối thiểu vẫn là 1. Câu trả lời đúng là 1. 

Một trường hợp khác là khi cửa sổ hợp lệ nhỏ nhất nằm ở cuối mảng, chẳng hạn như [1, 2, 3, 3, 4]. Thuật toán chỉ đạt hiệu lực khi quyền tới phần tử cuối cùng. Tại thời điểm đó, việc thu nhỏ chỉ loại bỏ các tiền tố dư thừa và phân đoạn tối thiểu cuối cùng sẽ trở thành [1, 2, 3, 4] một cách chính xác, mang lại độ dài 4. 

Trường hợp tinh tế cuối cùng là khi các bản sao được xen kẽ nhiều, chẳng hạn như [1, 2, 1, 2, 3]. Cửa sổ chỉ có hiệu lực ở phần tử cuối cùng và các cửa sổ một phần trước đó không bao giờ kích hoạt tính hợp lệ một cách sai lầm vì đã theo dõi sự hiện diện rõ ràng một cách chính xác, ngăn chặn các câu trả lời sớm không chính xác.
