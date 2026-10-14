---
title: "CF 105200A - Vấn đề về mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và nhiệm vụ là đánh giá lặp đi lặp lại một lượng "mảng con tối đa" cổ điển khi chúng ta mở rộng mảng từ trái sang phải."
date: "2026-06-27T02:52:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105200
codeforces_index: "A"
codeforces_contest_name: "IME++ Starters Try-outs 2024"
rating: 0
weight: 105200
solve_time_s: 55
verified: true
draft: false
---

[CF 105200A - Sự cố mảng](https://codeforces.com/problemset/problem/105200/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và nhiệm vụ là đánh giá lặp đi lặp lại một lượng "mảng con tối đa" cổ điển khi chúng ta mở rộng mảng từ trái sang phải. Tại mỗi tiền tố của mảng, chúng ta muốn biết tổng tốt nhất có thể của bất kỳ mảng con liền kề nào nằm hoàn toàn bên trong tiền tố đó. Đầu ra là chuỗi các giá trị tốt nhất này, một giá trị cho mỗi tiền tố. 

Một cách khác để đọc nhiệm vụ là chúng tôi mô phỏng việc hiển thị từng phần tử của mảng một lần. Sau khi nhìn thấy phần tử đầu tiên, chúng tôi tính tổng mảng con tối đa của phần tử đơn lẻ đó. Sau khi nhìn thấy hai phần tử đầu tiên, chúng tôi tính lại tổng mảng con tối đa trên cả hai. Điều này tiếp tục cho đến khi toàn bộ mảng được xử lý. 

Hàm ý ràng buộc chính là việc tính toán lại đơn giản cho mỗi tiền tố sẽ quá chậm. Nếu độ dài mảng đạt đến giới hạn lập trình cạnh tranh điển hình như 100000, thì việc tính toán lại toàn bộ quá trình quét Kadane cho mỗi tiền tố sẽ dẫn đến hành vi O(n²), điều này vượt xa mức chấp nhận được. Chúng ta cần duy trì trạng thái tăng dần để mỗi phần tử mới cập nhật câu trả lời theo thời gian không đổi. 

Một trường hợp cạnh phổ biến là khi tất cả các số đều âm. Ví dụ: nếu mảng là [-5, -2, -8] thì câu trả lời cho mỗi tiền tố là phần tử đơn lớn nhất được thấy cho đến nay, không phải bằng 0. Một triển khai Kadane ngây thơ đặt lại các số âm về 0 sẽ tạo ra các số 0 không chính xác, đây không phải là tổng của mảng con hợp lệ nếu các mảng con trống không được phép. 

Một trường hợp tinh tế khác là khi mảng con tốt nhất cho tiền tố kết thúc chính xác ở vị trí hiện tại. Ví dụ: trong [3, -10, 4], các câu trả lời tiền tố sẽ phát triển thành 3, 3, 4. Bất kỳ cách tiếp cận nào chỉ theo dõi mức tốt nhất toàn cầu mà không duy trì đúng trạng thái “kết thúc tốt nhất ở đây” sẽ không thành công trong quá trình chuyển đổi trong đó việc khởi động lại ở phần tử hiện tại sẽ tốt hơn là mở rộng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force tính toán lại tổng mảng con tối đa cho mỗi tiền tố một cách độc lập. Đối với mỗi tiền tố kết thúc tại chỉ mục i, chúng tôi quét tất cả các mảng con kết thúc tại hoặc trước i và tính tổng của chúng. Việc triển khai trực tiếp sẽ thử tất cả các cặp (l, r) với r ≤ i, dẫn đến O(n²) hoạt động trên mỗi tiền tố và O(n³) tổng thể nếu được triển khai một cách đơn giản thông qua phép tính tổng. Ngay cả khi tổng tiền tố giảm truy vấn tổng phạm vi xuống O(1), chúng tôi vẫn có mảng con O(n²) cho mỗi tiền tố, khiến việc này không khả thi. 

Lý do điều này gây lãng phí là vì các tiền tố liền kề có chung hầu hết cấu trúc. Khi chuyển từ tiền tố i sang tiền tố i+1, chúng tôi chỉ thêm một phần tử mới, nhưng brute Force sẽ tính toán lại mọi thứ từ đầu thay vì cập nhật kết quả trước đó. 

Quan sát quan trọng là thuật toán của Kadane đã duy trì chính xác hai thông tin cần thiết cho quá trình này: tổng mảng con tốt nhất kết thúc ở vị trí hiện tại và tổng mảng con tốt nhất được thấy cho đến nay. Giá trị đầu tiên có thể được cập nhật trong O(1) bằng cách quyết định xem nên mở rộng mảng con trước đó hay bắt đầu làm mới ở phần tử hiện tại. Giá trị thứ hai đơn giản là giá trị tối đa theo thời gian. 

Bằng cách duy trì hai trạng thái này khi lặp lại, chúng tôi tự nhiên tạo ra câu trả lời cho mọi tiền tố mà không cần tính toán lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2) hoặc O(n³) | O(1) | Quá chậm | 
| Kadane tối ưu cho mỗi tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo hai biến, một để theo dõi tổng mảng con tốt nhất kết thúc ở chỉ mục hiện tại và một để theo dõi câu trả lời tốt nhất được thấy cho đến nay. Phần tử đầu tiên phải bắt đầu từ phần tử đầu tiên vì không được phép có mảng con trống. 
2. Lặp lại mảng từ trái sang phải, xử lý từng phần tử một. Ở mỗi bước, chúng ta quyết định nên mở rộng mảng con trước đó hay bắt đầu một mảng con mới ở phần tử hiện tại. 
3. Cập nhật giá trị “kết thúc tốt nhất ở đây” là giá trị tối đa giữa riêng phần tử hiện tại và phần tử kết thúc tốt nhất trước đó cộng với phần tử hiện tại. Sự lựa chọn này phản ánh liệu sự liên tục có ích hay gây tổn hại. 
4. Cập nhật giá trị tốt nhất toàn cầu thành giá trị tối đa giữa giá trị hiện tại của nó và “kết thúc tốt nhất ở đây” mới được tính toán. 
5. Xuất kết quả tốt nhất toàn cục sau mỗi lần lặp, vì tại thời điểm đó, nó biểu thị tổng mảng con tối đa của tiền tố hiện tại. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến chính xác: sau khi xử lý chỉ mục i, biến theo dõi “kết thúc tốt nhất ở đây” bằng tổng tối đa của bất kỳ mảng con nào phải kết thúc chính xác tại i và giá trị tốt nhất toàn cục bằng tổng tối đa của bất kỳ mảng con nào có đầy đủ trong tiền tố [0, i]. Bất kỳ mảng con nào kết thúc tại i đều mở rộng mảng con kết thúc tại i-1 hoặc bắt đầu mới tại i, do đó quá trình chuyển đổi nắm bắt tất cả các khả năng. Vì mỗi mảng con có một vị trí kết thúc duy nhất nên mỗi mảng con ứng cử viên được xem xét chính xác một lần khi nó trở thành trạng thái “kết thúc tại đây”, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().strip().split()
    if not data:
        return
    n = int(data[0])
    arr = list(map(int, data[1:]))

    # In case input is split across lines
    if len(arr) < n:
        while len(arr) < n:
            arr.extend(map(int, input().split()))

    best_ending = arr[0]
    best_global = arr[0]

    out = [str(best_global)]

    for i in range(1, n):
        x = arr[i]

        best_ending = max(x, best_ending + x)
        best_global = max(best_global, best_ending)

        out.append(str(best_global))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc toàn bộ mảng một cách cẩn thận, bao gồm cả trường hợp dòng đầu tiên có thể không chứa tất cả các phần tử. Đây là một cạm bẫy lập trình cạnh tranh phổ biến khi đầu vào là khoảng trắng hoặc dòng mới được trộn lẫn. 

Biến`best_ending`tương ứng với tổng mảng con tốt nhất phải kết thúc ở vị trí hiện tại. bản cập nhật`max(x, best_ending + x)`trực tiếp mã hóa quyết định khởi động lại hoặc gia hạn. Biến`best_global`theo dõi những gì được thấy tốt nhất trên tất cả các tiền tố cho đến nay và được thêm vào sau mỗi lần cập nhật để tạo ra chuỗi đầu ra được yêu cầu. 

Một chi tiết tinh tế là khởi tạo. Cả hai biến đều bắt đầu ở phần tử đầu tiên, không phải 0, vì các mảng con phải chứa ít nhất một phần tử. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng [3, -2, 5]. 

| tôi | giá trị | kết thúc tốt nhất | tốt nhất_global | đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 3 | 3 | 
| 1 | -2 | max(-2, 3 + -2 = 1) = 1 | 3 | 3 | 
| 2 | 5 | tối đa(5, 1 + 5 = 6) = 6 | 6 | 3 3 6 | 

Dấu vết này cho thấy giá trị âm không đặt lại mọi thứ mà chỉ làm giảm tổng cuối cùng nếu phần mở rộng vẫn có lợi. Câu trả lời toàn cục vẫn ổn định cho đến khi một mảng con tốt hơn xuất hiện. 

Bây giờ hãy xem xét [-1, -2, -3]. 

| tôi | giá trị | kết thúc tốt nhất | tốt nhất_global | đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | -1 | -1 | -1 | -1 | 
| 1 | -2 | max(-2, -1 + -2 = -3) = -2 | -1 | -1 | 
| 2 | -3 | max(-3, -2 + -3 = -5) = -3 | -1 | -1 | 

Điều này xác nhận rằng thuật toán tránh được việc khởi tạo bằng 0 một cách chính xác và bảo toàn phần tử âm lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các lần chuyển đổi theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có hai biến đang chạy được duy trì | 

Độ phức tạp thời gian tuyến tính là cần thiết để xử lý các mảng lớn, vì bất kỳ phép tính lại bậc hai nào trên các tiền tố sẽ vượt quá giới hạn điển hình theo một số bậc độ lớn. Việc sử dụng không gian liên tục đảm bảo độ ổn định của bộ nhớ ngay cả ở kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# Since solve() prints directly, we adapt testing by capturing stdout
import contextlib

def run(inp: str) -> str:
    import sys, io
    backup_in = sys.stdin
    backup_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = backup_in
        sys.stdout = backup_out

# sample-like cases
assert run("3\n3 -2 5\n") == "3\n3\n6", "basic mixed case"
assert run("3\n-1 -2 -3\n") == "-1\n-1\n-1", "all negative"

# custom cases
assert run("1\n5\n") == "5", "single element"
assert run("5\n1 2 3 4 5\n") == "1\n3\n6\n10\n15", "increasing array"
assert run("4\n-1 5 -1 5\n") == "-1\n5\n5\n9", "alternating gains"
assert run("6\n2 -1 2 -1 2 -1\n") == "2\n2\n3\n3\n4\n4", "repeated interruptions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 5 | khởi tạo cơ sở | 
| mảng tăng dần | tăng trưởng tích lũy | hành vi chỉ tích cực | 
| lợi nhuận xen kẽ | lan truyền tối đa ổn định | ổn định tiền tố | 
| gián đoạn lặp đi lặp lại | khởi động lại và mở rộng logic | Chuyển tiếp Kadane | 

## Vỏ cạnh 

Đối với đầu vào như [-5, -1, -3], thuật toán khởi tạo bằng -5. Sau phần tử thứ hai, nó đánh giá best_ending = max(-1, -5 + -1 = -6) = -1 và best_global vẫn là -1. Sau phần tử thứ ba, best_ending = max(-3, -1 + -3 = -4) = -3 và best_global giữ nguyên -1. Đầu ra chính xác vẫn giữ nguyên giá trị tiền tố tốt nhất ở mỗi bước, không bao giờ đưa số 0 vào không chính xác. 

Đối với đầu vào như [4, -10, 6], trước tiên thuật toán đặt best_ending = 4, best_global = 4. Tại chỉ mục 1, best_ending trở thành max(-10, -6) = -6, nhưng best_global vẫn giữ nguyên 4. Tại chỉ mục 2, best_ending trở thành max(6, 0) = 6, cập nhật best_global lên 6. Điều này cho thấy cách khởi động lại ở phần tử thứ ba được ghi lại chính xác thông qua quy tắc chuyển đổi cục bộ thay vì bất kỳ phép tính lại toàn cục nào.
