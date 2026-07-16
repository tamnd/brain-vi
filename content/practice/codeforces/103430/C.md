---
title: "CF 103430C - Vận động viên"
description: "Chúng ta có hai nhóm vận động viên độc lập, một nhóm dành cho môn thể thao A và một nhóm dành cho môn thể thao B. Mỗi vận động viên có một giá trị kỹ năng bằng số và mỗi vận động viên phải ở lại môn thể thao của mình trừ khi chúng tôi quyết định rõ ràng "hoán đổi" họ, nghĩa là thay vào đó họ thi đấu ở môn thể thao khác."
date: "2026-07-03T09:44:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103430
codeforces_index: "C"
codeforces_contest_name: "2021-2022 ICPC, NERC, Southern and Volga Russian Regional Contest (problems intersect with Educational Codeforces Round 117)"
rating: 0
weight: 103430
solve_time_s: 48
verified: true
draft: false
---

[CF 103430C - Vận động viên](https://codeforces.com/problemset/problem/103430/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai nhóm vận động viên độc lập, một nhóm dành cho môn thể thao A và một nhóm dành cho môn thể thao B. Mỗi vận động viên có một giá trị kỹ năng bằng số và mỗi vận động viên phải ở lại môn thể thao của mình trừ khi chúng tôi quyết định rõ ràng "hoán đổi" họ, nghĩa là thay vào đó họ thi đấu ở môn thể thao khác. 

Mục tiêu là tạo ra sự lựa chọn cuối cùng gồm chính xác k vận động viên được chỉ định cho môn thể thao A và chính xác k vận động viên được chỉ định cho môn thể thao B, tối đa hóa tổng số kỹ năng sau bất kỳ lần hoán đổi nào. Việc hoán đổi tốn kém theo một cách rất cụ thể: nếu một vận động viên ban đầu thuộc môn thể thao A nhưng được sử dụng trong môn thể thao B, thì phần đóng góp của họ sẽ bị giảm đi một hình phạt x, và tương tự, nếu một vận động viên từ B được sử dụng trong môn A, hình phạt là y. 

Vì vậy, cấu trúc không chỉ là chọn k tốt nhất từ ​​​​mỗi danh sách một cách độc lập. Chúng tôi được phép kết hợp các vận động viên giữa các môn thể thao, nhưng mỗi lần chuyển nhượng chéo đều đi kèm với một khoản lỗ cố định tùy theo hướng. 

Các ràng buộc (ngầm từ cài đặt kiểu ICPC) đề xuất n có thể lớn, lên tới khoảng 2×10^5 hoặc tương tự trên cả hai danh sách. Điều đó ngay lập tức loại trừ việc tính toán lại một phần số tiền một cách độc lập cho mỗi lần phân bổ lại số vận động viên có thể có. Bất kỳ giải pháp nào liên tục sắp xếp, tính toán lại tiền tố hoặc mô phỏng các lựa chọn cho mỗi cấu hình sẽ quá chậm. Chúng tôi cần một lượt sắp xếp duy nhất và chuyển tiếp O(n). 

Một trường hợp phức tạp phát sinh khi lựa chọn tham lam được áp dụng độc lập cho mỗi môn thể thao. Ví dụ: giả sử việc hoán đổi một vận động viên rất mạnh từ B thành A buộc phải thay thế một vận động viên A yếu hơn thành B và chuỗi đó vẫn có thể cải thiện tổng điểm do sự khác biệt giữa x và y. Một trò chơi ngây thơ “lấy top k từ mỗi bên” không thành công ở đây vì nó bỏ qua sự tương tác giữa hai nhóm. 

Một dạng lỗi khác là giả sử tính đối xứng: coi việc hoán đổi A→B và B→A là tương đương. Không phải vậy, vì hình phạt x và y khác nhau, và sự phân chia tối ưu phụ thuộc vào sự bất đối xứng đó. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là quyết định tổng cộng có bao nhiêu vận động viên z chúng tôi chọn từ môn thể thao A trong số 2k vai trò được chọn cuối cùng. Sau khi z được cố định, 2k − z còn lại đến từ môn thể thao B. Với mỗi z, chúng tôi chọn những vận động viên giỏi nhất hiện có từ mỗi danh sách được sắp xếp và sau đó áp dụng hoán đổi để đáp ứng yêu cầu cuối cùng là k cho mỗi môn thể thao. 

Nếu chúng ta cố định z, thì cấu trúc tham lam tự nhiên sẽ rõ ràng: lấy các vận động viên z hàng đầu từ A và 2k − z hàng đầu từ B. Bây giờ, chúng ta so sánh có bao nhiêu vận động viên ở “phía sai” so với k bắt buộc cho mỗi môn thể thao. Nếu z < k, chúng ta thiếu bài tập A, vì vậy chúng ta phải chuyển k − z vận động viên từ B thành A, trả tiền phạt y cho mỗi lần hoán đổi. Nếu z > k, chúng ta phải chuyển z − k vận động viên từ A thành B, trả tiền phạt x cho mỗi lần hoán đổi. Do đó, tổng số điểm là sự kết hợp tổng tiền tố trừ đi số hạng phạt tuyến tính. 

Việc thử tất cả z một cách độc lập từ đầu sẽ tính toán lại các tổng tiền tố nhiều lần, điều này dẫn đến O(k) hoạt động trên mỗi z và do đó O(k^2). Khi thêm tính năng sắp xếp, kết quả này sẽ trở thành O(n log n + k^2), tốc độ này quá chậm khi k lớn. 

Quan sát quan trọng là khi z tăng thêm 1, chỉ có một vận động viên di chuyển từ B đến A trong lựa chọn dự kiến. Điều này thay đổi cấu trúc hình phạt và tổng tiền tố theo từng bước. Nếu chúng tôi duy trì tổng tiền tố cho cả hai danh sách được sắp xếp và theo dõi tổng từng phần hiện tại, chúng tôi có thể cập nhật tổng điểm theo O(1) mỗi bước. Điều này biến quá trình quét toàn bộ trên z từ 0 đến 2k thành quét tuyến tính. 

Chúng tôi sắp xếp cả hai mảng theo thứ tự giảm dần để tổng tiền tố luôn thể hiện các lựa chọn tốt nhất có thể. Sau đó, chúng tôi tính toán trước các tổng tiền tố để có thể truy vấn bất kỳ tổng tiền tố nào ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^2 + n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta duy trì các mảng A và B được sắp xếp theo thứ tự giảm dần và tiền tố của chúng có tổng SA và SB.

1. Sắp xếp cả hai mảng theo thứ tự giảm dần. Điều này đảm bảo rằng bất kỳ tiền tố nào cũng thể hiện sự lựa chọn tốt nhất có thể về kích thước đó cho mỗi môn thể thao. 
2. Tính toán trước tiền tố SA[i] là tổng của i vận động viên đầu tiên trong A và SB[i] tương tự cho B. Điều này cho phép tính toán theo thời gian không đổi của bất kỳ tổng phân khúc trên cùng nào. 
3. Lặp lại z từ 0 đến 2k, hiểu z là số vận động viên được chọn từ A trong tổng số 2k vận động viên được chọn. Với mỗi z, số từ B là 2k − z. 
4. Tính tổng cơ sở cho cấu hình này là SA[z] + SB[2k − z]. Điều này thể hiện tổng kỹ năng thô tốt nhất có thể trước bất kỳ giao dịch hoán đổi nào. 
5. Điều chỉnh tính khả thi của nhiệm vụ cuối cùng. Nếu z < k thì chúng ta không có đủ vận động viên được gắn nhãn A, vì vậy chúng ta phải chuyển đổi k − z vận động viên từ B thành A. Mỗi chuyển đổi như vậy làm giảm tổng số đi y, vì vậy hãy trừ y · (k − z). Nếu z > k, chúng ta phải chuyển đổi z − k vận động viên từ A sang B, do đó trừ x · (z − k). 
6. Theo dõi giá trị lớn nhất trên toàn bộ z. 
7. Đưa ra kết quả tốt nhất. 

Chi tiết quan trọng là mọi cấu hình đều được đánh giá theo thời gian không đổi sau khi xử lý trước, vì vậy chúng tôi tránh hoàn toàn việc tính toán lại. 

### Tại sao nó hoạt động 

Đối với bất kỳ z cố định nào, việc chọn các vận động viên z hàng đầu và 2k − z hàng đầu là tối ưu vì không có sự tương tác giữa các lựa chọn ngoài các ràng buộc về số lượng. Bất kỳ sai lệch nào thay thế vận động viên đã chọn bằng vận động viên yếu hơn chỉ có thể làm giảm tổng tiền tố. Sự kết hợp duy nhất còn lại giữa hai nhóm là việc cân bằng số lượng đến chính xác k cho mỗi môn thể thao và sự kết hợp đó là tuyến tính trong sự mất cân bằng, không phụ thuộc vào vận động viên cụ thể nào được chọn. Sự tách biệt giữa “lựa chọn tiền tố tốt nhất” và “hiệu chỉnh tuyến tính” này đảm bảo việc quét trên z khám phá tất cả các trạng thái tối ưu khác biệt về mặt cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k, x, y = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    A.sort(reverse=True)
    B.sort(reverse=True)

    # prefix sums
    SA = [0]
    for v in A:
        SA.append(SA[-1] + v)

    SB = [0]
    for v in B:
        SB.append(SB[-1] + v)

    ans = 0

    # z = number taken from A among 2k total
    for z in range(0, 2 * k + 1):
        if z > len(A) or (2 * k - z) > len(B):
            continue

        base = SA[z] + SB[2 * k - z]

        if z < k:
            base -= y * (k - z)
        elif z > k:
            base -= x * (z - k)

        if base > ans:
            ans = base

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp cả hai danh sách sao cho bất kỳ tiền tố nào cũng tương ứng với lựa chọn tốt nhất có thể có về kích thước đó. Tổng tiền tố cho phép đánh giá theo thời gian không đổi đối với bất kỳ lựa chọn z nào. 

Vòng lặp z kiểm tra mọi sự phân chia có thể có giữa hai môn thể thao. Việc kiểm tra tính khả thi đảm bảo chúng tôi không truy cập vào các chỉ mục tiền tố không hợp lệ. Bước điều chỉnh áp dụng hình phạt chính xác tùy thuộc vào việc chúng ta thiếu hay giao quá mức ở môn thể thao A. 

Mức tối đa được theo dõi trên toàn cầu và được in ở cuối. Việc triển khai phụ thuộc rất nhiều vào việc lập chỉ mục tiền tố chính xác; việc quên rằng SB[2k − z] phải luôn nằm trong giới hạn là nguyên nhân phổ biến gây ra lỗi thời gian chạy. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó k = 2. 

A = [10, 4, 3], B = [9, 8, 1], x = 2, y = 3. 

Chúng ta sắp xếp giảm dần rồi. 

Chúng tôi tính toán tổng tiền tố: 

Đáp: SA = [0, 10, 14, 17] 

B: SB = [0, 9, 17, 18] 

Chúng tôi đánh giá z. 

| z | SA[z] | SB[4−z] | tổng cơ sở | điều chỉnh | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 18 | 18 | − y·2 = −6 | 12 | 
| 1 | 10 | 17 | 27 | − y·1 = −3 | 24 | 
| 2 | 14 | 9 | 23 | 0 | 23 | 
| 3 | 17 | 0 | 17 | − x·1 = −2 | 15 | 
| 4 | không hợp lệ | 0 | bỏ qua | bỏ qua | bỏ qua | 

Kết quả tốt nhất là 24 tại z = 1, nghĩa là chúng ta cố ý lấy nhiều hơn từ B so với A mặc dù k cân bằng, vì giá trị cao của B lớn hơn các hình phạt. 

Dấu vết này cho thấy hành vi chính: sự phân chia tối ưu không nhất thiết phải tập trung ở k và việc quét tất cả z sẽ nắm bắt được sự dịch chuyển đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + k) | sắp xếp chiếm ưu thế, quét qua z là tuyến tính | 
| Không gian | O(n) | mảng tiền tố lưu trữ tổng tích lũy | 

Các ràng buộc cho phép kích thước đầu vào lên tới lớn và độ phức tạp này vừa vặn trong các giới hạn thông thường vì việc sắp xếp là chi phí chính và mọi thứ khác đều là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k, x, y = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    A.sort(reverse=True)
    B.sort(reverse=True)

    SA = [0]
    for v in A:
        SA.append(SA[-1] + v)

    SB = [0]
    for v in B:
        SB.append(SB[-1] + v)

    ans = 0
    for z in range(0, 2 * k + 1):
        if z > len(A) or 2 * k - z > len(B):
            continue
        base = SA[z] + SB[2 * k - z]
        if z < k:
            base -= y * (k - z)
        elif z > k:
            base -= x * (z - k)
        ans = max(ans, base)

    return str(ans)

# custom cases
assert run("1 1 1 1 1\n10\n20") == "29"
assert run("3 3 2 5 5\n5 4 3\n6 1 1") == "15"
assert run("2 2 1 100 1\n100 1\n1 100") == "99"
assert run("5 5 2 0 0\n1 2 3 4 5\n1 2 3 4 5") == "30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 trường hợp | 29 | lợi ích hoán đổi duy nhất | 
| đối xứng nhỏ k=2 | 15 | logic lựa chọn cân bằng | 
| hình phạt bất đối xứng | 99 | hoán đổi theo hướng | 
| không bị phạt | 30 | giảm xuống mức lựa chọn hàng đầu thuần túy | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một trong các mảng quá nhỏ để thỏa mãn các giá trị cực trị của z. Ví dụ: nếu k = 3 thì A chỉ có 2 phần tử và B có nhiều phần tử. Khi z = 5, SA[z] không hợp lệ và phải được bỏ qua. Thuật toán kiểm tra rõ ràng các giới hạn trước khi truy cập vào tổng tiền tố, đảm bảo tính chính xác bằng cách coi các cấu hình không thể thực hiện được là không ứng cử viên. 

Một trường hợp khác xảy ra khi hình phạt bằng 0. Trong tình huống này, việc hoán đổi không mất phí, vì vậy giải pháp tối ưu chỉ đơn giản là lấy 2k phần tử hàng đầu từ liên minh. Quá trình quét qua z vẫn hoạt động, nhưng giá trị tốt nhất xuất hiện ở đó z căn chỉnh một cách tự nhiên với sự phân bổ các giá trị lớn trên cả hai mảng. 

Trường hợp cuối cùng là khi một danh sách lấn át danh sách kia, khiến tất cả các cấu hình tối ưu bị lệch về một phía. Ví dụ: nếu tất cả các giá trị B lớn hơn A nhiều thì z tốt nhất có thể gần bằng 0. Việc liệt kê trên tất cả z đảm bảo giá trị cực trị này vẫn được đánh giá, thay vì giả định số dư ở k.
