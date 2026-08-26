---
title: "CF 104337A - Ma Thuật Thủ Tướng"
description: "Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng số nguyên. Mục tiêu là chuyển đổi từng mảng thành một chuỗi không giảm bằng cách sử dụng một loại thao tác đặc biệt và chúng tôi muốn thực hiện việc này bằng cách sử dụng càng ít thao tác càng tốt."
date: "2026-07-01T18:41:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104337
codeforces_index: "A"
codeforces_contest_name: "2023 Hubei Provincial Collegiate Programming Contest"
rating: 0
weight: 104337
solve_time_s: 60
verified: true
draft: false
---

[CF 104337A - Ma thuật tối thượng](https://codeforces.com/problemset/problem/104337/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng số nguyên. Mục tiêu là chuyển đổi từng mảng thành một chuỗi không giảm bằng cách sử dụng một loại thao tác đặc biệt và chúng tôi muốn thực hiện việc này bằng cách sử dụng càng ít thao tác càng tốt. 

Một thao tác hoạt động như sau: bạn chọn một đoạn liền kề có độ dài là số nguyên tố lẻ, sau đó bạn thêm +1 hoặc −1 vào mọi phần tử trong đoạn đó. Bạn có thể áp dụng nhiều thao tác như vậy, nhưng không được phép bất kỳ giá trị mảng nào trở thành âm. 

Đối với mỗi trường hợp thử nghiệm, đầu ra là số lượng tối thiểu các thao tác phân đoạn này cần thiết để làm cho mảng không giảm. 

Cấu trúc ràng buộc có vấn đề. Tổng độ dài trên tất cả các trường hợp thử nghiệm tối đa là 2×10^4, điều này cho thấy rõ ràng rằng chúng ta nên hướng tới hành vi gần như tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. Bất kỳ giá trị bậc hai nào trong kích thước mảng sẽ quá chậm nếu tất cả các trường hợp thử nghiệm đều lớn. 

Một cách giải thích ngây thơ sẽ cố gắng mô phỏng trực tiếp các hoạt động hoặc tìm kiếm mọi cách để áp dụng các phân đoạn. Điều đó ngay lập tức thất bại vì ngay cả với n = 2000, số lượng phân đoạn có thể là O (n^2) và mỗi phân đoạn có hai lựa chọn về dấu hiệu, khiến cho việc sử dụng vũ lực hoàn toàn không khả thi. 

Một vấn đề tế nhị cũng xuất phát từ ràng buộc “không có giá trị âm trong quá trình xử lý”. Một chiến lược tham lam bất cẩn, tự do trừ đi để sửa cấu trúc trong tương lai có thể vô tình tạo ra các trạng thái trung gian không hợp lệ ngay cả khi cấu hình cuối cùng có vẻ ổn. Ví dụ: việc giảm tiền tố để sửa lỗi đảo ngược cục bộ có thể tạm thời đẩy các giá trị xuống dưới 0 trước khi sửa chữa sau này. 

## Phương pháp tiếp cận 

Khó khăn chính là hiểu được hoạt động thực sự cho phép chúng ta làm gì trên toàn cầu. Mặc dù nó được mô tả như một bản cập nhật phân đoạn với độ dài hạn chế, nhưng quan sát quan trọng là chúng tôi không thực sự bị ràng buộc bởi độ dài nguyên tố cụ thể về mặt _tính biểu cảm_, mà chỉ về mặt chi phí. 

Quan điểm brute-force là coi mỗi thao tác như một công cụ có thể tăng hoặc giảm cục bộ một khối. Nếu chúng ta cố gắng tìm kiếm trực tiếp trình tự tối ưu của các hoạt động như vậy, chúng ta sẽ cần khám phá theo cấp số nhân nhiều trình tự của các phân đoạn chồng chéo. Ngay cả việc lập trình động theo các khoảng thời gian cũng sẽ thất bại vì quá trình chuyển đổi phụ thuộc vào sự thay đổi giá trị liên tục, không chỉ các trạng thái rời rạc. 

Cái nhìn sâu sắc về cấu trúc đến từ việc điều chỉnh lại mục tiêu. Chúng tôi không được yêu cầu tiếp cận một mảng mục tiêu cụ thể; chúng ta được yêu cầu thực thi một ràng buộc đơn điệu: mỗi vị trí ít nhất phải lớn bằng vị trí trước đó. Điều này biến vấn đề thành việc loại bỏ các “điểm rơi” trong mảng. 

Giả sử chúng ta quét từ trái sang phải. Bất cứ khi nào chúng ta gặp phải một vị trí mà`a[i] < a[i-1]`, chúng ta cần tăng giá trị tại vị trí`i`(và có thể cả những vị trí lân cận) đủ để nó không còn nhỏ hơn người tiền nhiệm của nó nữa. Bất kỳ chuỗi hoạt động hợp lệ nào cũng phải trả ít nhất “thâm hụt” này, bởi vì không có hoạt động nào có thể tránh khỏi việc sửa mọi vi phạm khi tiền tố giảm. 

Bây giờ hãy xem xét hoạt động của một phân đoạn +1 thực sự đóng góp như thế nào. Mặc dù nó ảnh hưởng đến một khối, chúng tôi có thể chọn các phân đoạn chồng chéo theo cách mà hiệu ứng thực có thể được tạo cục bộ: các cập nhật chồng chéo lặp đi lặp lại cho phép chúng tôi mô phỏng việc tăng đơn vị tại các vị trí riêng lẻ trong khi chỉ trả một thao tác cho mỗi đơn vị hiệu chỉnh. Việc hạn chế độ dài số nguyên tố lẻ không làm thay đổi kết luận này một cách tiệm cận, bởi vì chúng ta luôn có thể chọn các kích thước phân đoạn hợp lệ bao phủ vùng cục bộ được yêu cầu và chồng chéo chúng để tách biệt sự đóng góp hiệu quả. 

Điều này dẫn đến sự đơn giản hóa: chi phí chính xác là tổng số tiền tăng cần thiết để loại bỏ tất cả các bước đi xuống trong mảng khi quét từ trái sang phải. Mỗi lần chuỗi giảm xuống, chúng ta phải “nâng” hậu tố bắt đầu từ vị trí đó theo kích thước của lần giảm và mỗi lần nâng đơn vị tương ứng với một thao tác. 

Do đó, câu trả lời trở thành tổng của tất cả các khác biệt dương trong đó mảng giảm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua hoạt động | Hàm mũ | O(n) | Quá chậm | 
| Tiền tố sửa tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và tính toán số lượng sửa đổi đơn vị tối thiểu được yêu cầu. 

1. Bắt đầu từ phần tử thứ hai của mảng và so sánh nó với phần tử trước đó. 
2. Nếu giá trị hiện tại ít nhất bằng giá trị trước đó thì không cần hiệu chỉnh ở bước này vì thuộc tính không giảm không bị vi phạm ở đây. 
3. Nếu giá trị hiện tại nhỏ hơn giá trị trước đó, hãy tính chênh lệch`diff = a[i-1] - a[i]`. Đây là mức độ mà vị trí hiện tại phải được tăng lên để khôi phục tính đơn điệu tại thời điểm này. 
4. Thêm`diff`để trả lời. Điều này thể hiện số lượng đóng góp +1 đơn vị tối thiểu phải được áp dụng ở đâu đó bao gồm vị trí`i`để khắc phục vi phạm. 
5. Về mặt khái niệm, sau khi tính đến sự điều chỉnh này, hãy xử lý`a[i]`như được nâng lên để phù hợp`a[i-1]`để những so sánh trong tương lai có thể lan truyền một cách chính xác. 
6. Tiếp tục cho đến hết mảng. 

### Tại sao nó hoạt động 

Mỗi mức giảm giữa các phần tử liên tiếp thể hiện một lượng điều chỉnh tăng lên bắt buộc mà không thể tránh được bằng bất kỳ chuỗi hoạt động được phép nào. Vì mỗi đơn vị tăng phải đến từ một số thao tác nào đó nên tổng của tất cả các mức giảm là giới hạn dưới. 

Đồng thời, chúng ta có thể nhận ra giới hạn dưới này vì các hoạt động phân đoạn chồng chéo cho phép chúng ta phân phối mức tăng đơn vị mà không ảnh hưởng đến các phần đã cố định của mảng. Điều này có nghĩa là không cần phải "lãng phí" sự điều chỉnh đối với các phần tử đã hợp lệ và mỗi đơn vị tăng theo yêu cầu có thể được tính phí độc lập. 

Do đó, thuật toán tính toán cả số lượng hoạt động cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        ans = 0
        for i in range(1, n):
            if a[i] < a[i - 1]:
                ans += a[i - 1] - a[i]
                a[i] = a[i - 1]
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo sau quá trình quét trực tiếp từ trái sang phải. Mảng được cập nhật tại chỗ sao cho mỗi vị trí phản ánh giá trị đã sửa sau khi tính đến những thiếu hụt trước đó, điều này đảm bảo các so sánh sau này sử dụng tiền tố đã “cố định”. Điều này tránh việc đếm thiếu khi tích lũy nhiều giọt liên tiếp. 

Một lỗi triển khai phổ biến ở đây là quên cập nhật`a[i]`sau khi thêm sự khác biệt. Nếu không có bản cập nhật đó, những khác biệt sau này sẽ được đo lường so với mảng ban đầu thay vì tiến trình đã sửa, dẫn đến việc đếm hai lần hoặc đếm thiếu tùy thuộc vào kiểu giảm. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[1, 3, 2, 2, 5]`. 

Chúng tôi theo dõi quá trình chỉnh sửa từng bước. 

| tôi | một[i-1] | một [tôi] | khác biệt | trả lời | đã điều chỉnh a[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 0 | 0 | 3 | 
| 2 | 3 | 2 | 1 | 1 | 3 | 
| 3 | 3 | 2 | 1 | 2 | 3 | 
| 4 | 3 | 5 | 0 | 2 | 5 | 

Tổng đáp án là 2. Điều này tương ứng với việc cố định hai giọt nước ở vị trí 2 và 3. 

Bây giờ hãy xem xét một trường hợp giảm nghiêm ngặt`[5, 4, 3, 2]`. 

| tôi | một[i-1] | một [tôi] | khác biệt | trả lời | đã điều chỉnh a[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 4 | 1 | 1 | 5 | 
| 2 | 5 | 3 | 2 | 3 | 5 | 
| 3 | 5 | 2 | 3 | 6 | 5 | 

Kết quả tích lũy tất cả các phần thiếu hụt, cho thấy mỗi bước đóng góp độc lập vào số lượng hoạt động cần thiết. 

Những dấu vết này xác nhận rằng thuật toán tích lũy chính xác tổng “khối lượng đơn điệu bị mất” trong mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một đường chuyền từ trái sang phải với công việc không đổi trên mỗi phần tử | 
| Không gian | O(1) thêm | Chỉ duy trì bộ đếm đang chạy, mảng được sửa đổi tại chỗ | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là 2×10^4, do đó, việc quét tuyến tính cho mỗi trường hợp thử nghiệm dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = 0
        for i in range(1, n):
            if a[i] < a[i - 1]:
                ans += a[i - 1] - a[i]
                a[i] = a[i - 1]
        out.append(str(ans))
    return "\n".join(out)

# sample-style checks
assert run("1\n5\n1 3 2 2 5\n") == "2", "sample-like 1"

# already non-decreasing
assert run("1\n4\n1 2 3 4\n") == "0", "increasing array"

# strictly decreasing
assert run("1\n4\n5 4 3 2\n") == "6", "full decreasing"

# all equal
assert run("1\n5\n7 7 7 7 7\n") == "0", "flat array"

# single dip
assert run("1\n3\n10 1 10\n") == "9", "single large correction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 1 3 2 2 5 | 2 | mẫu hỗn hợp điển hình | 
| 1 4 1 2 3 4 | 0 | đã hợp lệ | 
| 1 4 5 4 3 2 | 6 | giọt tích lũy | 
| 1 5 7 7 7 7 7 | 0 | không cần thao tác | 
| 1 3 10 1 10 | 9 | nhúng sâu bị cô lập | 

## Vỏ cạnh 

Trường hợp cạnh hữu ích là một chuỗi xen kẽ lên xuống, chẳng hạn như`[1, 10, 1, 10, 1]`. Thuật toán xử lý từng giọt một cách độc lập, thêm`9`cho mỗi bước đi xuống. Điều này phù hợp với trực giác rằng mỗi hành vi vi phạm buộc phải thực hiện một sự điều chỉnh cục bộ hoàn toàn và các lần tăng sau đó sẽ không hủy bỏ các bản sửa lỗi bắt buộc trước đó. 

Một trường hợp khác là tiền tố giảm nghiêm ngặt dài, theo sau là tăng, ví dụ`[100, 90, 80, 90]`. Mức giảm lớn ban đầu sẽ tích lũy tất cả các điều chỉnh cần thiết ngay lập tức và mức tăng sau đó không làm giảm chi phí trước đó. Quá trình quét chính xác sẽ giữ tiền tố đã điều chỉnh ở giá trị tối đa cho đến nay, đảm bảo bước cuối cùng chỉ kiểm tra trạng thái đã được sửa. 

Cuối cùng, các chuỗi bắt đầu gần giới hạn dưới, chẳng hạn như`[1, 1, 1, 1]`xác nhận rằng không có giá trị âm nào được đưa ra hoặc cần thiết và thuật toán tự nhiên tạo ra các hoạt động bằng 0 mà không cần bất kỳ xử lý đặc biệt nào.
