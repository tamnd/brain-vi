---
title: "CF 104493F - Trò Chơi Cờ Bàn Mới"
description: "Chúng ta có một lưới $n lần n$ chứa đầy các số nguyên từ $1$ đến $n$. Tổng cộng mỗi giá trị xuất hiện chính xác $n$ lần, do đó lưới hoàn toàn cân bằng về mặt tần số, nhưng mặt khác thì tùy ý. Chúng tôi được phép áp dụng hai hoạt động toàn cầu bất kỳ số lần nào."
date: "2026-06-30T12:23:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104493
codeforces_index: "F"
codeforces_contest_name: "2023 ICPC HIAST Collegiate Programming Contest"
rating: 0
weight: 104493
solve_time_s: 50
verified: true
draft: false
---

[CF 104493F - Trò chơi cờ bàn mới](https://codeforces.com/problemset/problem/104493/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới chứa đầy các số nguyên từ$1$ĐẾN$n$. Mỗi giá trị xuất hiện chính xác$n$tổng số lần, do đó lưới được cân bằng hoàn hảo về mặt tần số, còn mặt khác thì tùy ý. 

Chúng tôi được phép áp dụng hai hoạt động toàn cầu bất kỳ số lần nào. Một thao tác dịch mỗi hàng sang phải một bước theo chu kỳ, và thao tác kia dịch mỗi cột xuống một bước theo chu kỳ. Các hoạt động này ảnh hưởng đến toàn bộ bảng một cách thống nhất, do đó chúng không sửa đổi các hàng hoặc cột một cách độc lập mà chỉ bằng các bản dịch tuần hoàn toàn cầu của lưới. 

Mục đích là để xác định liệu sau một số chuỗi thao tác này có thể có được một lưới “đẹp” hay không. Một lưới sẽ đẹp nếu mỗi hàng chứa mỗi số từ$1$ĐẾN$n$chính xác một lần và mỗi cột cũng chứa từng số từ$1$ĐẾN$n$đúng một lần. 

Những ràng buộc cho phép$n$lên tới 1000, có nghĩa là lưới có thể chứa tới một triệu ô. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng nhiều chuỗi hoạt động hoặc khám phá các trạng thái biến đổi khác nhau. Bất kỳ giải pháp đúng nào cũng phải kiểm tra lưới theo thời gian tuyến tính về cơ bản đối với kích thước của nó. 

Điểm tinh tế quan trọng là các phép toán có thể gợi ý một không gian biến đổi phức tạp, nhưng chúng thực sự có cấu trúc rất chặt chẽ: chúng chỉ thực hiện các phép dịch chuyển theo chu kỳ toàn cục. Một sai lầm phổ biến là cho rằng chúng ta có thể “sắp xếp lại” lưới điện một cách tự do hơn mức chúng ta thực sự có thể làm. 

Một vài trường hợp đặc biệt làm rõ cấu trúc: 

Nếu lưới đã là một hình vuông Latinh hợp lệ thì câu trả lời sẽ là CÓ, vì bất kỳ số lần dịch chuyển nào cũng bảo toàn thuộc tính. 

Nếu tất cả các hàng giống hệt nhau, ví dụ:```
1 2 3 4
1 2 3 4
1 2 3 4
1 2 3 4
```thì không có sự dịch chuyển toàn cục nào sẽ làm cho các cột có hoán vị hợp lệ, vì vậy câu trả lời là KHÔNG. 

Một sự hiểu lầm ngây thơ là nghĩ rằng chúng ta có thể căn chỉnh các hàng và cột một cách độc lập bằng cách sử dụng các ca. Điều đó là không thể vì cả hai hoạt động đều áp dụng trên toàn cầu và thống nhất. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi dịch chuyển hàng và cột có thể có. Mỗi thao tác sẽ thay đổi trạng thái lưới và chúng tôi có thể thử kiểm tra xem liệu bất kỳ trạng thái có thể truy cập nào có trở thành hình vuông Latinh hay không. Tuy nhiên, mặc dù chỉ có$n$những dịch chuyển có thể xảy ra theo từng hướng, việc kết hợp chúng sẽ dẫn đến$n^2$tiểu bang và mỗi chi phí kiểm tra$O(n^2)$, dẫn đến$O(n^4)$hành vi hoàn toàn không thể thực hiện được đối với$n = 1000$. 

Cái nhìn sâu sắc quan trọng là phải hiểu những hoạt động này thực sự làm gì. Việc dịch chuyển sang phải được áp dụng đồng thời cho tất cả các hàng tương đương với việc dịch chuyển theo chiều ngang theo chu kỳ của toàn bộ lưới. Dịch chuyển xuống được áp dụng cho tất cả các cột là dịch chuyển theo chiều dọc theo chu kỳ. Kết hợp chúng lại, bất kỳ chuỗi thao tác nào cũng dẫn đến sự dịch chuyển hình xuyến đồng đều của lưới: mọi ô đều được di chuyển bởi cùng một độ lệch$(\Delta r, \Delta c)$modulo$n$. 

Điều này có nghĩa là cấu trúc của mỗi hàng và mỗi cột, xét về giá trị nào xuất hiện và liệu có tồn tại trùng lặp hay không, là bất biến trong tất cả các thao tác được phép. Việc dịch chuyển không thể thêm hoặc loại bỏ các bản sao bên trong một hàng hoặc cột; nó chỉ sắp xếp lại vị trí. 

Do đó, nếu chúng ta hy vọng có được một lưới trong đó mỗi hàng và cột là một hoán vị thì các thuộc tính đó phải có trong lưới ban đầu. Các phép toán quá yếu để khắc phục bất kỳ vi phạm nào đối với các điều kiện bình phương Latinh. 

Vì vậy, vấn đề được rút gọn thành một phép xác thực đơn giản: kiểm tra xem mỗi hàng có chứa tất cả các số từ$1$ĐẾN$n$đúng một lần và mỗi cột cũng thỏa mãn điều kiện tương tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force theo ca |$O(n^4)$|$O(n^2)$| Quá chậm | 
| Kiểm tra hàng và cột |$O(n^2)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống việc xác minh thuộc tính hình vuông Latin. 

### bước 

1. Đọc lưới kích thước$n \times n$. 

Chúng tôi lưu trữ nó dưới dạng mảng 2D vì chúng tôi cần truy cập trực tiếp vào cả giá trị theo hàng và theo cột. 
2. Với mỗi hàng, hãy kiểm tra xem nó có chứa tất cả các số nguyên từ$1$ĐẾN$n$đúng một lần. 

Điều này được thực hiện bằng cách sử dụng mảng tần số hoặc mảng đánh dấu đã truy cập có kích thước$n$. 

Nếu bất kỳ số nào bị thiếu hoặc trùng lặp, lưới không bao giờ có thể hợp lệ vì các ca làm việc không thể sửa chữa cấu trúc hàng. 
3. Đối với mỗi cột, thực hiện kiểm tra tương tự. 

Điều này đảm bảo rằng không có giá trị nào lặp lại trong một cột và không có giá trị nào bị thiếu. 
4. Nếu cả kiểm tra hàng và kiểm tra cột đều đạt cho tất cả các chỉ mục, thì xuất CÓ. Nếu không thì xuất ra NO. 

Điều quan trọng là chúng tôi không bao giờ mô phỏng bất kỳ hoạt động nào. Chúng tôi chỉ xác minh xem lưới đã đáp ứng bất biến theo yêu cầu của trạng thái cuối cùng hay chưa. 

### Tại sao nó hoạt động 

Các hoạt động được phép là những thay đổi theo chu kỳ toàn cầu. Những sự dịch chuyển như vậy chỉ hoán đổi vị trí; chúng không thay đổi cấu trúc nhiều tập hợp bên trong bất kỳ hàng hoặc cột nào. Một hàng chứa các bản sao sẽ luôn chứa các bản sao sau bất kỳ chuỗi thao tác nào, bởi vì việc dịch chuyển sẽ duy trì mối quan hệ bình đẳng giữa các phần tử trong cùng một hàng. Điều tương tự cũng áp dụng cho các cột. 

Ngược lại, nếu mỗi hàng và mỗi cột đã tạo thành một hoán vị thì lưới đã là một hình vuông Latinh và bất kỳ sự dịch chuyển theo chu kỳ nào cũng bảo toàn thuộc tính đó, do đó một cấu hình hợp lệ sẽ tồn tại một cách tầm thường. 

Như vậy điều kiện vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_perm(arr, n):
    seen = [False] * (n + 1)
    for x in arr:
        if x < 1 or x > n:
            return False
        if seen[x]:
            return False
        seen[x] = True
    return True

def main():
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]

    for i in range(n):
        if not is_perm(a[i], n):
            print("NO")
            return

    for j in range(n):
        col = [a[i][j] for i in range(n)]
        if not is_perm(col, n):
            print("NO")
            return

    print("YES")

if __name__ == "__main__":
    main()
```Việc xác thực hàng được thực hiện trước tiên vì việc truy cập bộ nhớ theo tuần tự sẽ rẻ hơn một chút, cải thiện hoạt động của bộ nhớ đệm. Việc xác thực cột sẽ xây dựng từng cột một cách rõ ràng; đây vẫn là$O(n^2)$tổng thể và phù hợp thoải mái trong các hạn chế. 

Một điểm tinh tế là chúng tôi kiểm tra rõ ràng cả tính hợp lệ và trùng lặp của phạm vi. Mặc dù báo cáo vấn đề đảm bảo các giá trị nằm trong$1$ĐẾN$n$, việc giữ kiểm tra sẽ làm cho logic khép kín và tránh được các lỗi giả định ẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
3 1 2
2 3 1
```| Bước | Kiểm tra hàng | Kiểm tra cột | Quyết định | 
| --- | --- | --- | --- | 
| 1 | tất cả các hàng hợp lệ | đang chờ xử lý | tiếp tục | 
| 2 | tất cả các cột hợp lệ | tất cả đều hợp lệ | CÓ | 

Lưới này đã là một hình vuông Latinh nên mỗi hàng và cột đều chứa một hoán vị. Mọi ca chỉ xoay cấu trúc nên tính hợp lệ được bảo toàn. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
1 2 3 4
1 2 3 4
1 2 3 4
```| Bước | Kiểm tra hàng | Kiểm tra cột | Quyết định | 
| --- | --- | --- | --- | 
| 1 | hàng hợp lệ | đang chờ xử lý | tiếp tục | 
| 2 | cột 1 thất bại | được phát hiện | KHÔNG | 

Mỗi cột chứa các giá trị lặp lại, do đó, mặc dù các hàng là hoán vị nhưng điều kiện của cột sẽ thất bại ngay lập tức. Vì các ca thay đổi không thể thay đổi mẫu đẳng thức bên trong một cột nên không có cách nào để sửa cấu hình này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi ô được truy cập một lần để kiểm tra hàng và một lần để kiểm tra cột | 
| Không gian |$O(1)$phụ trợ (ngoài đầu vào) | Chỉ một mảng kích thước được truy cập có kích thước cố định$n$được sử dụng cho mỗi lần kiểm tra | 

Kích thước lưới lên tới một triệu ô và giải pháp này xử lý mỗi ô với số lần không đổi, dễ dàng phù hợp với giới hạn thời gian thông thường cho các giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def is_perm(arr, n):
        seen = [False] * (n + 1)
        for x in arr:
            if x < 1 or x > n:
                return False
            if seen[x]:
                return False
            seen[x] = True
        return True

    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]

    for i in range(n):
        if not is_perm(a[i], n):
            return "NO"

    for j in range(n):
        col = [a[i][j] for i in range(n)]
        if not is_perm(col, n):
            return "NO"

    return "YES"

# provided sample
assert run("""3
1 2 3
3 1 2
2 3 1
""") == "YES"

assert run("""4
1 2 3 4
1 2 3 4
1 2 3 4
1 2 3 4
""") == "NO"

# custom cases
assert run("""1
1
""") == "YES", "minimum size"

assert run("""2
1 2
2 1
""") == "YES", "already valid"

assert run("""3
1 1 1
2 2 2
3 3 3
""") == "NO", "row duplicates"

assert run("""3
1 2 3
1 2 3
3 1 2
""") == "NO", "mixed column failure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | CÓ | ranh giới tối thiểu | 
| trao đổi 2x2 | CÓ | hình vuông Latin hợp lệ đơn giản | 
| hàng lặp lại | KHÔNG | phát hiện trùng lặp hàng | 
| lưới hỗn hợp | KHÔNG | lan truyền lỗi cột | 

## Vỏ cạnh 

Lưới có kích thước tối thiểu$1 \times 1$luôn luôn vượt qua, vì hàng và cột duy nhất chứa hoán vị có độ dài 1. Thuật toán xử lý việc này một cách tự nhiên vì việc kiểm tra tần số thành công ngay lập tức. 

Lưới có các hàng đúng nhưng các cột thì không, chẳng hạn như các hàng giống hệt nhau xếp chồng lên nhau theo chiều dọc, sẽ không thành công trong giai đoạn xác thực cột. Thuật toán xây dựng rõ ràng từng cột và phát hiện các bản sao, do đó không cần chuyển đổi để phát hiện vấn đề. 

Lưới có các cột đúng nhưng các hàng không đối xứng và không thành công ở bước xác thực hàng trước tiên. Việc thoát sớm này sẽ ngăn cản việc tính toán không cần thiết. 

Một hình vuông Latinh hoàn toàn hợp lệ vẫn hợp lệ trong tất cả các lần kiểm tra, xác nhận rằng thuật toán không từ chối các cấu hình chính xác.
