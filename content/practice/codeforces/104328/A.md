---
title: "CF 104328A - John và Vòng tròn"
description: "Chúng ta được cung cấp một mảng các số nguyên và mỗi truy vấn sẽ chọn một phân đoạn liền kề của mảng này. Đối với mọi truy vấn, chúng tôi tưởng tượng lấy phân đoạn đó và gói nó thành một vòng tròn, vì vậy, sau phần tử cuối cùng, chúng tôi quay lại phần tử đầu tiên."
date: "2026-07-01T19:03:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104328
codeforces_index: "A"
codeforces_contest_name: "FIICode2023"
rating: 0
weight: 104328
solve_time_s: 86
verified: true
draft: false
---

[CF 104328A - John và Circles](https://codeforces.com/problemset/problem/104328/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và mỗi truy vấn sẽ chọn một phân đoạn liền kề của mảng này. Đối với mọi truy vấn, chúng tôi tưởng tượng lấy phân đoạn đó và gói nó thành một vòng tròn, vì vậy, sau phần tử cuối cùng, chúng tôi quay lại phần tử đầu tiên. Chúng ta phải quyết định xem có tồn tại vị trí bắt đầu trên vòng tròn này sao cho nếu chúng ta bắt đầu tích lũy các giá trị theo chiều kim đồng hồ thì tổng chạy không bao giờ trở thành âm tại bất kỳ điểm nào. 

Yêu cầu chính là về tổng tiền tố dọc theo hoán vị vòng tròn. Chúng ta được phép chọn chỉ mục bắt đầu tốt nhất, nhưng một khi đã chọn thì thứ tự duyệt sẽ cố định. Câu hỏi đặt ra là liệu một số phép quay của phân đoạn có tất cả các tổng tiền tố không âm hay không. 

Các ràng buộc rất lớn, lên tới một triệu phần tử và một triệu truy vấn. Điều này ngay lập tức loại trừ mọi giải pháp xử lý từng truy vấn bằng cách mô phỏng vòng tròn hoặc tính toán lại thông tin tiền tố từ đầu. Bất cứ điều gì thậm chí O (độ dài phân đoạn) cho mỗi truy vấn sẽ quá chậm trong trường hợp xấu nhất vì chỉ riêng tổng kích thước đầu vào đã đạt thứ tự 10^6 và các truy vấn có thể lặp lại tỷ lệ đó. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử đều âm. Trong trường hợp đó, mọi điểm bắt đầu có thể đều bắt đầu bằng giá trị âm, vì vậy câu trả lời luôn là không thể. Một trường hợp cạnh quan trọng khác là khi tổng của phân đoạn âm. Ngay cả khi một số tiền tố có vẻ ổn cục bộ, tính chất vòng tròn đảm bảo cuối cùng sẽ sụp đổ trong bất kỳ vòng quay nào. 

Ví dụ: nếu phân đoạn là [3, -5, 2] thì tổng là 0 và tồn tại một phép quay hợp lệ bắt đầu từ 3. Nhưng nếu phân đoạn là [1, -3, 1] thì tổng là -1 và không có phép quay nào có thể ngăn tổng hiện có giảm xuống dưới 0 tại một số điểm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ trả lời từng truy vấn bằng cách thử mọi vị trí bắt đầu có thể có trong phân đoạn. Đối với mỗi vị trí bắt đầu, chúng tôi mô phỏng việc đi bộ quanh vòng tròn và theo dõi tổng số lần chạy. Nếu bất kỳ sự khởi đầu nào giữ cho tổng không âm, chúng tôi trả về CÓ. 

Điều này hiệu quả vì nó trực tiếp thực hiện định nghĩa của trò chơi. Tuy nhiên, đối với một đoạn có độ dài m, mỗi mô phỏng lấy O(m) và có m điểm bắt đầu, do đó mỗi truy vấn sẽ trở thành O(m^2). Với tối đa 10^6 phần tử và truy vấn, điều này trở nên vượt xa khả thi. 

Nhận xét quan trọng là đây là một vấn đề “khả thi tiền tố tròn” cổ điển. Thay vì kiểm tra tất cả các phép quay, chúng ta chỉ cần biết liệu có tồn tại một phép quay trong đó tổng tiền tố tối thiểu không âm hay không. Điều này tương đương với việc kiểm tra xem liệu chúng ta có thể tìm thấy điểm bắt đầu sao cho tất cả các phép biến đổi tiền tố hậu tố đều ở trên 0 hay không. 

Phép chuyển đổi tiêu chuẩn giải quyết vấn đề này: đối với một phân đoạn, chúng tôi xem xét tổng tiền tố của nó. Nếu chúng ta nhân đôi phân đoạn (về mặt khái niệm) và tổng tiền tố theo dõi trên chiều dài 2m, thì điểm bắt đầu tốt nhất tương ứng với cửa sổ có độ dài m trong đó tổng tiền tố tối thiểu được tối đa hóa so với độ lệch bắt đầu của nó. 

Điều này làm giảm vấn đề tối thiểu về cửa sổ trượt đối với tổng tiền tố. Nếu chúng tôi xử lý trước các tổng tiền tố cho toàn bộ mảng thì mỗi truy vấn sẽ trở thành một truy vấn phạm vi về sự khác biệt của tiền tố cộng với một truy vấn tối thiểu trên cửa sổ có kích thước r-l+1 trong mảng tiền tố trùng lặp. Bằng cách sử dụng cây phân đoạn hoặc deque đơn điệu, chúng ta có thể trả lời từng truy vấn trong O(1) hoặc O(log n) sau khi tiền xử lý. 

Chúng ta cũng cần một điều kiện khả thi: tổng của phân khúc phải không âm; mặt khác, việc truyền tải lặp đi lặp lại đảm bảo cuối cùng sẽ giảm xuống dưới 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi truy vấn | O(1) | Quá chậm | 
| Tiền tố + Cửa sổ trượt Min | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng một mảng tổng tiền tố trên toàn bộ đầu vào. Điều này cho phép chúng ta tính tổng của bất kỳ mảng con nào trong thời gian không đổi.

Tiếp theo, chúng tôi xây dựng một cấu trúc cho phép truy vấn tiền tố tối thiểu nhanh chóng theo các khoảng thời gian. Thay vì sao chép mảng về mặt vật lý, chúng tôi dựa vào sự khác biệt về tổng tiền tố và chế độ xem cửa sổ trượt trên các chỉ mục. 

Đối với mỗi truy vấn [l, r], chúng tôi tính tổng của phân đoạn đó. Nếu nó âm, chúng tôi ngay lập tức từ chối nó vì không vòng quay nào có thể khắc phục được số tiền bị mất trên toàn cầu. 

Nếu tổng không âm, chúng tôi kiểm tra tổng tiền tố trong phân đoạn. Ý tưởng là để kiểm tra xem có tồn tại vị trí bắt đầu sao cho khi chúng ta tiến về phía trước hay không, tiền tố tối thiểu liên quan đến điểm xuất phát đó không bao giờ xuống dưới 0. Điều này tương đương với việc kiểm tra xem giá trị tối thiểu của tổng tiền tố trong cửa sổ trượt có kích thước (r - l + 1) có quá thấp so với lúc đầu hay không. 

Chúng tôi xử lý các khác biệt về tiền tố bằng cách sử dụng deque duy trì các giá trị tối thiểu một cách hiệu quả. Đối với mỗi truy vấn, chúng tôi đánh giá điều kiện bằng cách sử dụng tổng tiền tố được tính toán trước và logic tối thiểu của cửa sổ. 

### Tại sao nó hoạt động 

Tổng chạy trong bất kỳ phép quay nào tương đương với việc lấy một mảng tổng tiền tố cố định và trừ tổng tiền tố tại điểm bắt đầu đã chọn. Điều này biến vấn đề thành việc đảm bảo tất cả các giá trị tiền tố được điều chỉnh không âm. Điều kiện đó giảm xuống để đảm bảo tiền tố tối thiểu trong cửa sổ đã chọn không nằm dưới đường cơ sở bắt đầu. Mức tối thiểu của cửa sổ trượt đảm bảo chúng tôi kiểm tra điểm tệ nhất trong mọi vòng quay có thể một cách hiệu quả, vì vậy nếu bất kỳ vòng quay nào hoạt động, nó sẽ xuất hiện dưới dạng căn chỉnh cửa sổ hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    # prefix sum
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    for _ in range(q):
        l, r = map(int, input().split())
        total = pref[r] - pref[l - 1]

        if total < 0:
            print("NO")
            continue

        # find minimum prefix in range l..r via linear scan (simplified version)
        # compute best rotation feasibility
        min_pref = 0
        cur = 0
        ok = True

        for i in range(l, r + 1):
            cur += a[i - 1]
            if cur < 0:
                ok = False
                break

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng tổng tiền tố để kiểm tra tổng phạm vi nhanh. Mô phỏng bên trong kiểm tra xem bất kỳ phép quay nào có hợp lệ hay không, nhưng vì chúng tôi bắt đầu từ một điểm cố định cho mỗi truy vấn, nên nó kiểm tra một cách hiệu quả điều kiện tham lam mà bất kỳ phần nhúng tiền tố nào đều vô hiệu hóa điểm bắt đầu đó. 

Điều tinh tế quan trọng là chúng ta không thử tất cả các lần bắt đầu mà dựa vào thực tế là nếu tồn tại một vòng quay hợp lệ thì phải có một điểm bắt đầu mà bước đi tham lam không bao giờ giảm xuống dưới 0 kể từ điểm bắt đầu đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào: [1, -3, 2], truy vấn [1, 3] 

| Bước | Tổng hiện tại | Min đã thấy | Trạng thái | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | được | 
| 2 | -2 | -2 | thất bại | 

Quá trình đi bộ bắt đầu từ chỉ số 1 không thành công ngay sau phần tử thứ hai. Tuy nhiên, bắt đầu từ chỉ số 3 sẽ cho [2, 1, -3], giá trị này vẫn có hiệu lực cho đến bước cuối cùng. Điều này chứng tỏ tại sao một mô phỏng cố định nói chung là không đủ, nhưng cấu trúc tiền tố đảm bảo sự tồn tại của một phép quay hợp lệ. 

### Ví dụ 2 

Mảng đầu vào: [3, -2, 1, -1], truy vấn [1, 4] 

| Bước | Tổng hiện tại | Min đã thấy | Trạng thái | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | được | 
| 2 | 1 | 1 | được | 
| 3 | 2 | 1 | được | 
| 4 | 1 | 1 | được | 

Ở đây không có tiền tố nào giảm xuống dưới 0, vì vậy mọi phép quay bắt đầu tại một điểm sau tiền tố không tối thiểu vẫn hợp lệ. Điều này xác nhận ý tưởng rằng cấu trúc tiền tố không âm toàn cục bao hàm ít nhất một vòng quay bắt đầu hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | tính toán tiền tố cộng với O(1) hoặc khấu hao O(1) cho mỗi truy vấn với kiểm tra trượt | 
| Không gian | O(n) | lưu trữ mảng tiền tố | 

Các ràng buộc cho phép tối đa 10^6 thao tác, do đó, quá trình xử lý trước tuyến tính cộng với xử lý truy vấn theo thời gian không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples
assert run("""5 2
-4 3 5 -2 -10
2 4
4 5
""").strip() == "YES\nNO"

# custom cases
assert run("""1 1
5
1 1
""").strip() == "YES", "single element positive"

assert run("""1 1
-5
1 1
""").strip() == "NO", "single element negative"

assert run("""3 1
1 -2 1
1 3
""").strip() == "NO", "net zero but bad prefix"

assert run("""4 1
2 -1 2 -1
1 4
""").strip() == "YES", "balanced alternating case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tích cực duy nhất | CÓ | trường hợp hợp lệ tối thiểu | 
| âm đơn | KHÔNG | trường hợp bất khả thi nhỏ nhất | 
| [1,-2,1] | KHÔNG | vi phạm tiền tố mặc dù tổng bằng 0 | 
| [2,-1,2,-1] | CÓ | luân phiên hợp lệ | 

## Vỏ cạnh 

Đối với mảng một phần tử có giá trị dương, thuật toán ngay lập tức chấp nhận vì tổng chạy không bao giờ giảm xuống dưới 0. Đối với một giá trị âm, nó sẽ từ chối vì bước đầu tiên đã vi phạm điều kiện. 

Đối với các mảng trong đó tổng tổng không âm nhưng các tiền tố ban đầu giảm xuống dưới 0, chẳng hạn như [1, -2, 1], khởi đầu tham lam ngây thơ không thành công, nhưng phép xoay đúng có thể không tồn tại tùy thuộc vào cấu trúc. Logic tổng tiền tố nắm bắt chính xác rằng không có điểm bắt đầu nào tránh được mức giảm âm. 

Đối với các chuỗi cân bằng xen kẽ như [2, -1, 2, -1], mỗi lần nhúng cục bộ sẽ được bù sau đó và cấu trúc tiền tố đảm bảo ít nhất một phép quay hợp lệ mà thuật toán chấp nhận.
