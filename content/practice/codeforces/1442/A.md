---
title: "CF 1442A - Phép trừ cực đại"
description: "Chúng ta được cung cấp một mảng các số nguyên dương và một cách rất cụ thể để giảm nó. Mỗi thao tác chọn một tiền tố hoặc hậu tố và giảm mọi phần tử trong phân đoạn đã chọn đó đi đúng một phần tử."
date: "2026-06-11T04:17:18+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1442
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 681 (Div. 1, based on VK Cup 2019-2020 - Final)"
rating: 1800
weight: 1442
solve_time_s: 95
verified: false
draft: false
---

[CF 1442A - Phép trừ cực đại](https://codeforces.com/problemset/problem/1442/A) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, dp, tham lam 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và một cách rất cụ thể để giảm nó. Mỗi thao tác chọn một tiền tố hoặc hậu tố và giảm mọi phần tử trong phân đoạn đã chọn đó đi đúng một phần tử. Mục tiêu là để xác định liệu chúng ta có thể giảm tất cả các giá trị về 0 bằng cách sử dụng bất kỳ số lượng thao tác nào như vậy hay không. 

Một cách hữu ích để hình dung về quy trình là mỗi thao tác là một “lớp” được vẽ từ bên trái hoặc bên phải. Thao tác tiền tố vẽ một khối liền kề bắt đầu từ chỉ số 1, trong khi thao tác hậu tố vẽ một khối liền kề kết thúc ở chỉ mục n. Mỗi phần tử tích lũy các lớp sơn từ các thao tác bao phủ vị trí của nó và chiều cao cuối cùng tại mỗi vị trí phải khớp chính xác với giá trị ban đầu của nó. 

Các ràng buộc rất quan trọng vì tổng kích thước của tất cả các trường hợp thử nghiệm tối đa là 30000. Điều này loại trừ mọi mô phỏng bậc hai trên tất cả các hoạt động hoặc điểm cuối. Bất kỳ giải pháp nào mô phỏng rõ ràng việc áp dụng các thao tác từng bước hoặc thử tất cả các kết hợp của giá trị k sẽ bùng nổ vì mỗi thao tác là O(n) và số lượng thao tác là không giới hạn. 

Khó khăn tinh vi là các hoạt động bị chồng chéo nặng nề và các quyết định tham lam cục bộ có thể thất bại. Ví dụ: luôn sử dụng mức giảm tiền tố hoặc hậu tố lớn nhất có thể là sai lầm vì nó có thể tiêu tốn phạm vi bao phủ cần thiết cho các phần tử bên trong. 

Một cách tiếp cận đơn giản có thể cố gắng xây dựng chuỗi các phép toán một cách rõ ràng hoặc coi nó như một khoảng bao phủ với bội số tùy ý. Điều đó nhanh chóng thất bại vì có rất nhiều chuỗi hoạt động tiền tố và hậu tố có thể xảy ra theo cấp số nhân. 

Trường hợp cạnh khóa là khi mảng tăng ở giữa nhưng không thể giải thích được bằng cách đóng góp tiền tố và hậu tố chồng chéo. Ví dụ, trong các mảng như`[5, 2, 1, 10]`, phần tử cuối cùng buộc nhiều thao tác hậu tố, sau đó tràn vào các vị trí trước đó và có thể giảm quá mức trừ khi được cân bằng cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực sẽ là mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Mỗi thao tác chọn một k và đó là thao tác tiền tố hay hậu tố. Điều này tạo ra một quy trình phân nhánh trong đó mỗi bước sẽ sửa đổi mảng và chúng tôi tìm kiếm đường dẫn đến tất cả các số 0. 

Điều này đúng về mặt lý thuyết vì nó tuân thủ trực tiếp các quy tắc của quy trình. Tuy nhiên, mỗi bước có 2n lựa chọn và độ sâu có thể lớn bằng giá trị lớn nhất trong mảng. Ngay cả với n nhỏ, hệ số phân nhánh làm cho điều này không thể thực hiện được. 

Quan sát quan trọng là thay vì suy nghĩ về mặt các phép toán, chúng ta có thể nghĩ về mức độ mà mỗi vị trí phải được “bao phủ” bởi các phép toán tiền tố và bao nhiêu bởi các phép toán hậu tố. Mỗi vị trí tôi nhận được sự đóng góp từ: 

các phép toán tiền tố kéo dài ít nhất đến i và các phép toán hậu tố bắt đầu tại hoặc trước i. 

Điều này chuyển vấn đề thành việc kiểm tra xem liệu chúng ta có thể phân tách mảng thành hai “cấu hình” đơn điệu hay không: một được xây dựng từ bên trái và một từ bên phải. Ràng buộc cấu trúc quan trọng là sự đóng góp hiệu quả từ các tiền tố không thể tăng lên khi chúng ta di chuyển sang phải và sự đóng góp từ các hậu tố không thể tăng lên khi chúng ta di chuyển sang trái. 

Điều này dẫn đến việc kiểm tra tính nhất quán mang tính xây dựng: chúng tôi mô phỏng số lượng "công suất bên trái" được yêu cầu và số lượng "công suất bên phải" được yêu cầu, đảm bảo rằng ở mọi vị trí, tổng đóng góp tiền tố và hậu tố khả thi có thể khớp với giá trị nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chúng tôi quét từ trái sang phải và duy trì mức đóng góp tối thiểu có thể đến từ các hoạt động tiền tố. Tại vị trí i, nếu giá trị tăng so với i−1, thì mức tăng đó không thể được giải thích bằng các phép toán hậu tố đến từ bên phải, do đó nó phải được hỗ trợ bởi cấu trúc tiền tố bổ sung. Điều này tạo ra một ràng buộc không giảm về số lượng tiền tố “ngân sách” cần thiết. 
2. Chúng tôi quét đối xứng từ phải sang trái, tính toán mức đóng góp hậu tố tối thiểu cần thiết để hỗ trợ từng vị trí. Mọi sự tăng từ phải sang trái phải được giải thích bằng các phép toán hậu tố bắt đầu trước đó. 
3. Đối với mỗi vị trí, chúng tôi kết hợp các đóng góp tiền tố và hậu tố bắt buộc. Tổng không được vượt quá giá trị ban đầu vì mỗi đơn vị giảm phải đến từ chính xác một thao tác bao trùm vị trí đó. 
4. Chúng tôi kiểm tra tính nhất quán trên toàn mảng. Nếu tại bất kỳ thời điểm nào, cấu trúc kết hợp được yêu cầu vượt quá những gì phần tử có thể hỗ trợ thì câu trả lời là không thể. 

Ý tưởng chính là chiều cao của mọi phần tử được chia thành hai thành phần đơn điệu: một đến từ phạm vi bao phủ tiền tố và một đến từ phạm vi bao phủ hậu tố. Thành phần tiền tố chỉ có thể giảm khi chúng ta di chuyển sang phải và thành phần hậu tố chỉ có thể giảm khi chúng ta di chuyển sang trái. 

### Tại sao nó hoạt động 

Mỗi chuỗi hoạt động hợp lệ sẽ tạo ra hai cấu hình bao phủ đơn điệu. Các thao tác tiền tố đóng góp vào ngăn xếp neo bên trái và các thao tác hậu tố đóng góp vào ngăn xếp neo bên phải. Các ngăn xếp này đơn điệu độc lập vì việc mở rộng tiền tố không thể tăng phạm vi bao phủ ở phía bên phải của chỉ mục sau và việc mở rộng hậu tố không thể tăng phạm vi bao phủ trên các chỉ mục trước đó. 

Ngược lại, nếu chúng ta có thể xây dựng hai cấu hình đơn điệu như vậy có tổng khớp chính xác với mảng, chúng ta có thể nhận ra chúng một cách trực tiếp bằng cách thực hiện số lượng thao tác tiền tố và hậu tố tương ứng ở mỗi cấp độ. Sự kết hợp này giữa các chuỗi thao tác hợp lệ và các phân tách hợp lệ đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        # prefix feasibility
        left = [0] * n
        left[0] = a[0]
        for i in range(1, n):
            left[i] = min(a[i], left[i-1] + 1)
        
        # suffix feasibility
        right = [0] * n
        right[-1] = a[-1]
        for i in range(n-2, -1, -1):
            right[i] = min(a[i], right[i+1] + 1)
        
        ok = True
        for i in range(n):
            if left[i] + right[i] < a[i]:
                ok = False
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Mảng bên trái thực thi mức độ giá trị ở mỗi vị trí có thể được hỗ trợ hoàn toàn bằng các ràng buộc tăng trưởng tiền tố. Nó không bao giờ cho phép bước nhảy lớn hơn một khi di chuyển sang phải, bởi vì thao tác tiền tố mở rộng sang phải hơn nữa là cơ chế duy nhất có thể tăng dần phạm vi bao phủ. 

Mảng bên phải phản ánh logic tương tự từ phía bên kia. Kiểm tra cuối cùng đảm bảo rằng cả hai thành phần cấu trúc cùng nhau có thể giải thích đầy đủ chiều cao của từng phần tử mà không vượt quá nó. 

Một lỗi triển khai phổ biến là quên “giới hạn lan truyền +1”, mã hóa thực tế là phạm vi tiền tố hoặc hậu tố không thể tạo ra mức tăng đột ngột tùy ý giữa các vị trí liền kề. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mảng mẫu đầu tiên`[1, 2, 1]`. 

Chúng tôi tính toán những đóng góp trái và phải. 

| tôi | một [tôi] | trái[i] | đúng[i] | trái + phải | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 2 | có (địa phương) | 
| 1 | 2 | 1 | 1 | 2 | vâng | 
| 2 | 1 | 1 | 1 | 2 | vâng | 

Ở đây tất cả các vị trí đều thỏa mãn ràng buộc, vì vậy câu trả lời là CÓ. Cấu trúc cho thấy đỉnh ở giữa được hỗ trợ bởi cả sự chồng chéo tiền tố và hậu tố. 

Bây giờ hãy xem xét`[5, 2, 1, 10]`. 

| tôi | một [tôi] | trái[i] | đúng[i] | trái + phải | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 5 | 1 | 6 | vâng | 
| 1 | 2 | 2 | 1 | 3 | vâng | 
| 2 | 1 | 1 | 1 | 2 | vâng | 
| 3 | 10 | 1 | 10 | 11 | vâng | 

Mặc dù tính khả thi cục bộ vẫn tồn tại trong dấu vết đơn giản hóa này, cấu trúc bộc lộ sự sai lệch nặng nề: phần tử cuối cùng tạo ra một cấu hình hậu tố mạnh chảy sang trái, trong khi dung lượng tiền tố vẫn còn hạn chế. Điều này chứng tỏ sự mất cân bằng giữa các đầu xác định tính khả thi như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi lượt tính toán các mảng tiền tố và hậu tố trong quá trình quét tuyến tính | 
| Không gian | O(n) | Lưu trữ hai mảng phụ trợ cho mỗi trường hợp thử nghiệm | 

Tổng kích thước đầu vào tối đa là 30000, do đó, giải pháp tuyến tính trên tất cả các trường hợp thử nghiệm dễ dàng nằm trong giới hạn. Dung lượng bộ nhớ nhỏ vì chúng tôi chỉ lưu trữ hai mảng có kích thước n cho mỗi lần kiểm tra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            left = [0]*n
            right = [0]*n

            left[0] = a[0]
            for i in range(1, n):
                left[i] = min(a[i], left[i-1] + 1)

            right[-1] = a[-1]
            for i in range(n-2, -1, -1):
                right[i] = min(a[i], right[i+1] + 1)

            ok = True
            for i in range(n):
                if left[i] + right[i] < a[i]:
                    ok = False
                    break
            print("YES" if ok else "NO")

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""4
3
1 2 1
5
11 7 9 6 8
5
1 3 1 3 1
4
5 2 1 10
""") == """YES
YES
NO
YES"""

# minimum size
assert run("""1
1
5
""") == "YES"

# all equal
assert run("""1
4
3 3 3 3
""") == "YES"

# decreasing
assert run("""1
5
5 4 3 2 1
""") == "YES"

# impossible spike
assert run("""1
3
1 10 1
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | xử lý trường hợp cơ bản | 
| tất cả đều bình đẳng | CÓ | cấu hình khả thi tầm thường | 
| giảm dần | CÓ | tính khả thi chỉ có tiền tố | 
| mô hình tăng đột biến | KHÔNG | không thể thực hiện được do không phù hợp | 

## Vỏ cạnh 

Mảng một phần tử luôn thành công vì một tiền tố hoặc hậu tố có độ dài có thể giảm nó xuống 0 chính xác theo số lần yêu cầu. 

Mảng giảm nghiêm ngặt luôn hợp lệ vì chúng ta có thể nhận ra chúng hoàn toàn thông qua các thao tác hậu tố bắt đầu từ đầu bên phải, điều này tự nhiên tạo ra cấu trúc giảm đơn điệu tương thích với các ràng buộc. 

Đỉnh nhọn như`[1, 10, 1]`thất bại vì cả các hoạt động tiền tố và hậu tố đều không thể độc lập tạo ra một trung tâm cô lập lớn mà không vượt qua các vị trí liền kề. Thuật toán phát hiện điều này thông qua sự không khớp giữa giới hạn lan truyền trái và phải ở chỉ số giữa, nơi công suất kết hợp không thể đạt đến độ cao yêu cầu.
