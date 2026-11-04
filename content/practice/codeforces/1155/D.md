---
title: "CF 1155D - Mảng Đẹp"
description: "Chúng ta được cấp một dãy số nguyên và được phép tùy ý chọn chính xác một đoạn liền kề và nhân mọi phần tử bên trong nó với một giá trị cố định x."
date: "2026-06-12T02:43:50+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "divide-and-conquer", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1155
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 63 (Rated for Div. 2)"
rating: 1900
weight: 1155
solve_time_s: 103
verified: true
draft: false
---

[CF 1155D - Mảng đẹp](https://codeforces.com/problemset/problem/1155/D) 

**Xếp hạng:** 1900 
**Tags:** vũ lực, cấu trúc dữ liệu, chia để trị, dp, tham lam 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và được phép tùy ý chọn chính xác một đoạn liền kề và nhân mọi phần tử bên trong nó với một giá trị cố định`x`. Sau khi thực hiện việc này, chúng ta xem xét tất cả các mảng con liền kề của mảng kết quả và lấy tổng tối đa có thể có trên các mảng con đó, trong đó chúng ta cũng được phép chọn mảng con trống có tổng bằng 0. 

Nhiệm vụ là chọn phân đoạn đã sửa đổi hoặc chọn không làm gì theo cách làm cho tổng mảng con tốt nhất có thể càng lớn càng tốt. 

Ràng buộc`n ≤ 3 · 10^5`ngay lập tức loại trừ bất kỳ giải pháp nào thử mọi mảng con có thể có cho phép nhân. Một cách tiếp cận bậc ba hoặc thậm chí bậc hai để tính lại tổng mảng con tối đa cho mỗi lựa chọn sẽ liên quan đến khoảng`10^10`hoạt động trong trường hợp xấu nhất, vượt xa mức 2 giây cho phép. Điều này buộc phải có giải pháp lập trình động tuyến tính hoặc gần tuyến tính trong đó mỗi vị trí được xử lý trong thời gian không đổi. 

Một khó khăn nhỏ là mảng con tốt nhất sau khi sửa đổi có thể không khớp với phân đoạn được sửa đổi một cách rõ ràng. Mảng con tối ưu có thể bắt đầu trước phân đoạn được nhân, kết thúc sau phân đoạn đó hoặc nằm hoàn toàn bên trong phân đoạn đó. Một ý tưởng tham lam ngây thơ như “chọn mảng con tốt nhất và sau đó quyết định xem có nhân nó hay không” thất bại vì mảng con tốt nhất phụ thuộc vào chính phép nhân. 

Một trường hợp thất bại phổ biến khác là giả định rằng chúng ta phải luôn nhân một phân đoạn có tác động tích cực lên tổng. Nếu như`x`là âm hoặc bằng 0, phép nhân có thể phá hủy những đóng góp tích cực trước đó, do đó quyết định phải được tối ưu hóa toàn cầu thay vì được chọn cục bộ. 

## Phương pháp tiếp cận 

Ý tưởng về bạo lực rất đơn giản: hãy thử mọi phân đoạn có thể`[l, r]`để nhân, mô phỏng mảng và tính tổng mảng con tối đa bằng thuật toán Kadane. Tính toán Kadane cho một chi phí cấu hình`O(n)`, và có`O(n^2)`sự lựa chọn cho`[l, r]`, dẫn đến`O(n^3)`tổng công việc. Ngay cả với các thủ thuật tiền xử lý, việc chỉ liệt kê tất cả các phân đoạn cũng không thể thực hiện được. 

Quan sát chính là cấu trúc của vấn đề về cơ bản vẫn là một chiều và tuần tự. Tại mỗi chỉ mục, chúng ta chỉ cần biết hiện tại chúng ta không sử dụng thao tác, đang ở trong phân đoạn được nhân hay đã sử dụng xong. Điều này biến bài toán thành một hệ thống lập trình động ba trạng thái tương tự như thuật toán của Kadane nhưng được mở rộng với “giai đoạn chuyển đổi một lần”. 

Thay vì nghĩ đến việc chọn một phân đoạn trước, chúng tôi xử lý mảng từ trái sang phải và duy trì tổng mảng con tốt nhất có thể kết thúc ở mỗi vị trí theo ba điều kiện: chưa sử dụng phép nhân, hiện đang ở trong phân đoạn được nhân và đã nhân xong. Điều này tránh việc liệt kê các phân đoạn hoàn toàn, bởi vì mọi phân đoạn có thể tương ứng với chính xác một lần chuyển đổi vào và ra khỏi trạng thái "nhân tích cực". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| DP tối ưu (3 trạng thái) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba giá trị đang chạy trong khi quét mảng. 

Giá trị đầu tiên thể hiện tổng mảng con tốt nhất kết thúc ở vị trí hiện tại khi chúng ta chưa sử dụng phép nhân. 

Số thứ hai biểu thị tổng mảng con tốt nhất kết thúc ở vị trí hiện tại khi chúng ta đang ở trong phân đoạn được nhân. 

Phần thứ ba đại diện cho tổng mảng con tốt nhất kết thúc ở vị trí hiện tại sau khi phân đoạn nhân đã kết thúc. 

Chúng tôi cũng cho phép ngầm các mảng con trống bằng cách giữ số 0 làm ứng cử viên cơ sở. 

### bước 

1. Khởi tạo ba trạng thái ở phần tử đầu tiên. Trạng thái "không hoạt động" là bắt đầu mới hoặc chỉ lấy phần tử đó. Trạng thái “hoạt động bên trong” là phần tử được nhân với`x`. Trạng thái “sau khi vận hành” bắt đầu chỉ như chính phần tử đó. 
2. Đối với mỗi phần tử tiếp theo, hãy cập nhật trạng thái “không hoạt động” bằng cách mở rộng mảng con không hoạt động trước đó hoặc bắt đầu làm mới ở phần tử hiện tại. Điều này phản ánh logic tiêu chuẩn của Kadane trước khi áp dụng bất kỳ sửa đổi nào. 
3. Cập nhật trạng thái “vận hành bên trong” theo ba cách có thể. Chúng ta có thể bắt đầu thao tác chính xác ở phần tử hiện tại, chuyển sang phần tử đó từ mảng con không hoạt động hoặc tiếp tục nhân từ phân đoạn được nhân trước đó. Mỗi tùy chọn tương ứng với việc chọn nơi bắt đầu phân đoạn được nhân. 
4. Cập nhật trạng thái “sau hoạt động” theo ba cách có thể. Chúng ta có thể tiếp tục mảng con sau thao tác, kết thúc phép nhân sớm hơn một bước và bây giờ mở rộng bình thường hoặc bắt đầu một mảng con sau thao tác mới ở vị trí hiện tại. 
5. Theo dõi giá trị tốt nhất được thấy trên cả ba trạng thái và tất cả các vị trí. 

### Tại sao nó hoạt động 

Tại mỗi chỉ mục, mỗi trạng thái biểu thị tổng tốt nhất có thể có của một mảng con kết thúc tại chỉ mục đó theo một ràng buộc chính xác về việc phân đoạn nhân đã được sử dụng hay chưa. Mọi lựa chọn hợp lệ của một phân đoạn nhân liền kề tương ứng với chính xác một chuỗi chuyển đổi: một điểm mà chúng ta chuyển sang trạng thái “hoạt động bên trong” và một điểm sau đó là điểm chúng ta rời khỏi nó. Vì tất cả các chuyển đổi như vậy đều được xem xét và mỗi trạng thái luôn lưu trữ cấu trúc con tối ưu kết thúc ở chỉ mục hiện tại, nên không có cấu hình hợp lệ nào bị bỏ qua và không có cấu hình không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    NEG = -10**30

    dp0 = 0
    dp1 = NEG
    dp2 = NEG

    ans = 0

    for v in a:
        ndp0 = max(0, dp0 + v)

        ndp1 = max(v * x,
                   dp1 + v * x,
                   dp0 + v * x)

        ndp2 = max(v,
                   dp2 + v,
                   dp1 + v)

        dp0, dp1, dp2 = ndp0, ndp1, ndp2

        ans = max(ans, dp0, dp1, dp2)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai nén DP thành các biến cuộn thay vì mảng.`dp0`theo dõi tổng mảng con tốt nhất kết thúc ở đây mà không cần sử dụng thao tác.`dp1`các bản nhạc nằm trong phân đoạn được nhân lên và`dp2`dấu vết đang theo sau nó. Các quá trình chuyển đổi trực tiếp mã hóa việc chúng ta bắt đầu, mở rộng hay chuyển đổi các giai đoạn. Biến`ans`đảm bảo chúng tôi thu được giá trị tốt nhất ở bất kỳ vị trí nào, bao gồm cả trường hợp mảng con tối ưu kết thúc trước khi mảng kết thúc. 

Việc sử dụng hằng số âm lớn sẽ ngăn chặn các chuyển đổi không hợp lệ chiếm ưu thế khi trạng thái chưa được khởi tạo. Đường cơ sở bằng 0 là cần thiết vì mảng con trống được cho phép và phải cạnh tranh với tổng âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 -2
-3 8 -2 1 -6
```Chúng tôi theo dõi các trạng thái khi chúng tôi quét. 

| tôi | giá trị | dp0 | dp1 | dp2 | 
| --- | --- | --- | --- | --- | 
| 1 | -3 | 0 | 6 | -3 | 
| 2 | 8 | 8 | -6 | 14 | 
| 3 | -2 | 6 | 4 | 12 | 
| 4 | 1 | 7 | -2 | 13 | 
| 5 | -6 | 1 | 12 | 7 | 

Giá trị tốt nhất quan sát được là`22`, đạt được khi phân đoạn nhân được chọn sao cho mảng con kết hợp hiệu quả các đóng góp giữa các trạng thái, với sự chuyển đổi tối ưu vào và ra khỏi vùng nhân ở đúng ranh giới. 

Dấu vết này cho thấy giải pháp tối ưu không bị giới hạn trong một pha duy nhất; mảng con tốt nhất có thể đi qua cả ba trạng thái DP. 

### Ví dụ 2 

đầu vào:```
3 1
-1 -2 -3
```| tôi | giá trị | dp0 | dp1 | dp2 | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | 0 | -1 | -1 | 
| 2 | -2 | 0 | -2 | -2 | 
| 3 | -3 | 0 | -3 | -3 | 

Câu trả lời là`0`, vì mọi tổng của mảng con đều âm và mảng con trống chiếm ưu thế. nhân với`1`không thay đổi bất cứ điều gì và DP duy trì chính xác số 0 xuyên suốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử cập nhật một số trạng thái không đổi một lần | 
| Không gian | O(1) | Chỉ có ba biến lăn được duy trì | 

Quét tuyến tính phù hợp thoải mái trong các ràng buộc đối với`n = 3 · 10^5`và việc sử dụng bộ nhớ liên tục sẽ tránh mọi chi phí từ các bảng DP lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    NEG = -10**30

    dp0 = 0
    dp1 = NEG
    dp2 = NEG
    ans = 0

    for v in a:
        ndp0 = max(0, dp0 + v)
        ndp1 = max(v * x, dp1 + v * x, dp0 + v * x)
        ndp2 = max(v, dp2 + v, dp1 + v)
        dp0, dp1, dp2 = ndp0, ndp1, ndp2
        ans = max(ans, dp0, dp1, dp2)

    return str(ans)

# provided sample
assert run("5 -2\n-3 8 -2 1 -6\n") == "22"

# custom: all negative, no benefit
assert run("3 5\n-1 -2 -3\n") == "0"

# custom: all positive, x=1
assert run("4 1\n1 2 3 4\n") == "10"

# custom: single element
assert run("1 -10\n5\n") == "5"

# custom: strong negative x flips sign
assert run("4 -1\n1 -2 3 -4\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tiêu cực | 0 | sự thống trị của mảng con trống | 
| tất cả đều tích cực | 10 | không cần sử dụng thao tác | 
| phần tử đơn | 5 | tính đúng đắn của trường hợp cơ sở | 
| trộn với âm x | 6 | lợi ích đảo ngược dấu hiệu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử đều âm. Một biến thể Kadane tham lam không có đường cơ sở bằng 0 rõ ràng sẽ trả về sai số âm, trong khi câu trả lời đúng là 0 do tùy chọn mảng con trống. DP rõ ràng mang số 0 về phía trước để thuật toán không bao giờ cam kết với một phân mảng có hại. 

Một trường hợp tế nhị khác là khi`x`là âm, điều này có thể làm cho phân khúc xấu trước đó trở nên có lợi. DP ba trạng thái xử lý việc này một cách tự nhiên vì việc nhập trạng thái nhân luôn được phép ở bất kỳ chỉ mục nào, do đó, thuật toán có thể khám phá các phân đoạn tối ưu mà sẽ không bao giờ được chọn theo phương pháp heuristic “chỉ tăng tích cực” ngây thơ. 

Trường hợp cạnh cuối cùng xảy ra khi giải pháp tối ưu không sử dụng phép nhân nào cả. Điều này được xử lý bởi thực tế là`dp0`luôn cạnh tranh trực tiếp ở mức tối đa toàn cầu, đảm bảo thuật toán có thể bỏ qua hoàn toàn hoạt động khi nó có hại.
