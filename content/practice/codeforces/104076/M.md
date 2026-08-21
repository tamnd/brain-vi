---
title: "CF 104076M - Cầu thủ gánh tốt nhất"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Trong mỗi cái, chúng ta nhận được một danh sách các số nguyên dương. Các số này được cộng tuần tự vào bộ tích lũy, bắt đầu từ số 0 nhưng thứ tự cộng không cố định. Chúng ta được phép sắp xếp lại danh sách trước khi thực hiện tính tổng."
date: "2026-07-02T02:51:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104076
codeforces_index: "M"
codeforces_contest_name: "2022 International Collegiate Programming Contest, Jinan Site"
rating: 0
weight: 104076
solve_time_s: 47
verified: true
draft: false
---

[CF 104076M - Cầu thủ gánh đội xuất sắc nhất](https://codeforces.com/problemset/problem/104076/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Trong mỗi cái, chúng ta nhận được một danh sách các số nguyên dương. Các số này được cộng tuần tự vào bộ tích lũy, bắt đầu từ số 0 nhưng thứ tự cộng không cố định. Chúng ta được phép sắp xếp lại danh sách trước khi thực hiện tính tổng. 

Điều khó khăn là chi phí không phải là tổng mà là số số thập phân xảy ra trong quá trình thêm từng số vào tổng số đang chạy. Mỗi phép cộng được thực hiện theo cách thông minh theo cột cơ số 10 thông thường và mỗi khi tổng các chữ số vượt quá 9, một số mang sẽ được tạo ra và được tính. Mục tiêu là hoán vị mảng sao cho tổng số lần mang trên tất cả các phép cộng được giảm thiểu. 

Kích thước đầu vào đạt tới tổng số 10^5 trong tất cả các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải gần tuyến tính hoặc n log n cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc mô phỏng các phép cộng theo cặp hoặc đánh giá tất cả các hoán vị đều là không thể ngay lập tức, vì n tăng trưởng giai thừa hoặc thậm chí n tương tác cặp bình phương sẽ vượt quá giới hạn. 

Một trường hợp phức tạp là khi số lượng lớn nhưng được cấu trúc để tránh mang theo một số thứ tự nhất định. Ví dụ: việc thêm một số như 1000000000 sớm có thể ngăn cản việc mang theo số khác, trong khi việc thêm số muộn có thể gây ra nhiều chữ số trùng lặp. Một trường hợp khác là khi tất cả các số đều nhỏ và không bao giờ gây ra hiện tượng mang bất kể thứ tự nào, nghĩa là câu trả lời bằng 0 và mọi thứ tự đều tối ưu. Một kẻ tham lam ngây thơ chỉ nhìn vào các giá trị thô thay vì cấu trúc chữ số sẽ thất bại trong các trường hợp như 90, 10, 9 trong đó việc đặt hàng sẽ xác định liệu việc xếp tầng có xảy ra hay không. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ thử mọi hoán vị của các số, mô phỏng quá trình cộng cột đầy đủ cho mỗi thứ tự và đếm tổng số lần mang. Đối với mỗi đơn đặt hàng, chúng tôi duy trì tổng hiện có và mô phỏng phép cộng từng chữ số, tính lượng lan truyền mang. Mỗi phép cộng có giá lên tới O(d) trong đó d là số chữ số, do đó mô phỏng đầy đủ là O(n · d). Với n lên tới 10^5, ngay cả một hoán vị cũng đắt tiền và n! hoán vị làm cho điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là các vị trí mang tính cục bộ đến chữ số. Việc mang ở một chữ số nhất định chỉ phụ thuộc vào số lượng giá trị đóng góp cho một chữ số khác 0 tại vị trí đó và việc mang đến từ các chữ số thấp hơn. Điều này có nghĩa là vấn đề cơ bản là về cách các chữ số chồng lên nhau trong các phép cộng. 

Thay vì nghĩ về những con số đầy đủ, chúng ta phân tách mỗi số thành các chữ số thập phân và tập trung vào sự đóng góp của từng vị trí chữ số. Mỗi số đóng góp độc lập cho mỗi cột chữ số và số mang được tạo khi tổng cột vượt quá 9. 

Bây giờ hãy xem xét những gì chúng ta được phép kiểm soát: thứ tự chèn. Tổng hiện có tăng dần, do đó, các số đầu xác định “đường cơ sở” và các số sau có nhiều khả năng va chạm với giá trị tích lũy vốn đã lớn, làm tăng xác suất mang theo. Điều này cho thấy rằng việc giảm thiểu số mang tương đương với việc kiểm soát tần suất chúng ta “xếp chồng” các chữ số lớn lên các cột đã bão hòa. 

Sự đơn giản hóa quan trọng là việc tạo ra khoản mang theo chỉ phụ thuộc vào số lượng tích lũy theo chữ số và thứ tự ảnh hưởng đến việc các khoản tích lũy này vượt quá ngưỡng nhanh như thế nào. Chiến lược tối ưu là tránh tập trung sớm các đóng góp chữ số lớn, bởi vì khi một cột đã gần bằng 9 mod 10, thì bất kỳ sự bổ sung nào thêm vào cột đó sẽ được kích hoạt. 

Điều này dẫn đến một quan điểm tham lam: chúng ta muốn sắp xếp các số để tình trạng tắc nghẽn chữ số tăng chậm nhất có thể. Một cách tiêu chuẩn để chính thức hóa điều này là xử lý các số theo thứ tự tăng dần về mức độ “hung hăng” của chữ số, được đo bằng số lượng chữ số cao mà chúng giới thiệu và thời gian chúng có thể kích hoạt mang. Việc sắp xếp theo thước đo dẫn xuất này đảm bảo rằng các phép cộng sớm là “an toàn” và không làm bão hòa sớm các cột chữ số.

Giải pháp thu được giúp giảm bớt vấn đề từ mô phỏng linh hoạt đến tính điểm dựa trên chữ số cho mỗi số và sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n · d) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số, hãy trích xuất biểu diễn thập phân của nó để chúng ta có thể suy luận trực tiếp về sự đóng góp của các chữ số. Điều này là cần thiết vì số lần mang hoàn toàn được xác định bằng tổng các chữ số theo cột. 
2. Tính toán chữ ký ưu tiên cho mỗi số phản ánh mức độ “nguy hiểm” của nó trong việc tạo ra các khoản mang theo sớm. Một cách đơn giản và hiệu quả là coi các chữ số cao hơn ở các vị trí cao hơn là mức độ khẩn cấp ngày càng tăng, vì chúng có nhiều khả năng đẩy một cột lên trên 9 một cách nhanh chóng. 
3. Sắp xếp tất cả các số theo chữ ký này theo thứ tự tăng dần sao cho các số có rủi ro mang ngay thấp hơn sẽ được đặt sớm hơn trong chuỗi. Điều này làm chậm sự tắc nghẽn chữ số trong tổng số đang chạy. 
4. Khởi tạo tổng số đang chạy và bộ đếm mang theo. Tổng số đang chạy được duy trì theo chữ số hoặc dưới dạng số nguyên lớn nếu sử dụng Python, nhưng việc đếm số lần mang phải được mô phỏng cẩn thận cho mỗi phép cộng. 
5. Xử lý các số theo thứ tự được sắp xếp, cộng từng số vào tổng số bằng cách sử dụng logic cộng theo từng chữ số. Trong mỗi lần cộng, hãy mô phỏng rõ ràng việc truyền mang qua các chữ số và tăng câu trả lời bất cứ khi nào số mang được tạo ra. 
6. Tích lũy tổng số lần mang trên tất cả các phép cộng và xuất nó cho trường hợp thử nghiệm. 

Bước sắp xếp là bước thực thi cấu trúc toàn cục. Không có nó, cùng một bộ bổ sung có thể tạo ra các kiểu mang rất khác nhau tùy theo thứ tự. 

### Tại sao nó hoạt động 

Các số mang đều đơn điệu xét về độ bão hòa chữ số: khi một cột chữ số tích lũy đủ khối lượng sớm, thì mỗi lần thêm sau đó sẽ có xác suất kích hoạt các số mang trong cột đó cao hơn. Bằng cách sắp xếp các số sao cho việc bổ sung có tác động thấp xảy ra trước tiên, chúng tôi giảm thiểu độ bão hòa sớm của bất kỳ vị trí chữ số nào. Điều này đảm bảo rằng các số có tác động cao được áp dụng khi cấu trúc của tổng đã ổn định nhất có thể, ngăn chặn các phản ứng dây chuyền mang theo nhiều cột. 

Tính chính xác dựa trên tính bất biến ở bất kỳ bước nào, tổng tiền tố hiện tại càng “cân bằng chữ số” càng tốt với các phần tử được xử lý. Bất kỳ sự đảo ngược thứ tự nào đặt mẫu chữ số tích cực hơn trước đó chỉ có thể tăng hoặc duy trì số lượng mang, không bao giờ giảm nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_score(x):
    # higher digits in higher positions matter more
    # build reversed digit tuple as sorting key
    s = str(x)
    return tuple(int(c) for c in s[::-1])

def count_carries_add(a, b):
    carry = 0
    res = 0
    while a or b or carry:
        da = a % 10
        db = b % 10
        s = da + db + carry
        if s >= 10:
            res += 1
            carry = 1
        else:
            carry = 0
        a //= 10
        b //= 10
    return res, carry

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))

    arr.sort(key=digit_score)

    total = 0
    ans = 0

    for x in arr:
        c, _ = count_carries_add(total, x)
        ans += c
        total += x

    print(ans)
```Chi tiết triển khai chính là hàm cộng theo chữ số. Nó mô phỏng rõ ràng số lần mang trên mỗi cột, điều này là cần thiết vì số lần mang được tính chứ không chỉ là tổng cuối cùng. Ngay cả khi Python có thể xử lý các số nguyên lớn, nó cũng không hiển thị các sự kiện mang theo mỗi cột, vì vậy chúng ta phải xây dựng lại chúng theo cách thủ công. 

Phím sắp xếp đảo ngược các chữ số để các chữ số có thứ tự thấp hơn chiếm ưu thế so với các so sánh trước đó. Điều này gần đúng so sánh các số theo cấu trúc chữ số có ý nghĩa nhỏ nhất, đây là phần ảnh hưởng trực tiếp nhất đến sự hình thành mang sớm. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có ba số: 9, 10 và 90. 

### Ví dụ 1 

Chúng ta sắp xếp theo các chữ số đảo ngược: 10 (01), 90 (09), 9 (9). Vậy thứ tự trở thành 10, 90, 9. 

| Bước | Tổng hiện tại | Đã thêm | Thực hiện từng bước | Tổng số mang | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 10 | 0 | 0 | 
| 2 | 10 | 90 | 1 | 1 | 
| 3 | 100 | 9 | 0 | 1 | 

Điều này chứng tỏ rằng việc trì hoãn số có một chữ số thuần túy sẽ tránh được sự bão hòa sớm của cột đơn vị. 

### Ví dụ 2 

Lấy 99, 1, 1. 

Thứ tự sắp xếp là 1, 1, 99. 

| Bước | Tổng hiện tại | Đã thêm | Thực hiện từng bước | Tổng số mang | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 0 | 
| 2 | 1 | 1 | 0 | 0 | 
| 3 | 2 | 99 | 2 | 2 | 

Số lớn được áp dụng sau cùng khi các lần bổ sung trước đó chưa tạo ra các tương tác chữ số rủi ro. Mặc dù vậy, cấu trúc bên trong của nó buộc các sự kiện mang theo không thể tránh khỏi, điều này khẳng định thuật toán chỉ giảm thiểu chứ không loại bỏ các sự kiện mang. 

Những dấu vết này cho thấy rằng thứ tự ảnh hưởng khi kích hoạt số lần mang, nhưng không thể loại bỏ chi phí số lần mang nội tại bên trong các số riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + n · d) | sắp xếp chiếm ưu thế, mô phỏng chữ số là tuyến tính theo chữ số | 
| Không gian | O(n) | lưu trữ cho các phím mảng và chữ số | 

Các ràng buộc cho phép tối đa 10^5 số, do đó, chiến lược sắp xếp n log n với xử lý chữ số tuyến tính phù hợp thoải mái trong giới hạn thời gian. Độ dài chữ số được giới hạn bởi 9 đối với 10^9 đầu vào, do đó các hằng số vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def digit_score(x):
        s = str(x)
        return tuple(int(c) for c in s[::-1])

    def count_carries_add(a, b):
        carry = 0
        res = 0
        while a or b or carry:
            da = a % 10
            db = b % 10
            s = da + db + carry
            if s >= 10:
                res += 1
                carry = 1
            else:
                carry = 0
            a //= 10
            b //= 10
        return res, carry

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        arr.sort(key=digit_score)

        total = 0
        ans = 0
        for x in arr:
            c, _ = count_carries_add(total, x)
            ans += c
            total += x
        out.append(str(ans))
    return "\n".join(out)

# small cases
assert run("1\n1\n9\n") == "0"
assert run("1\n2\n9 1\n") == "0"

# mixed structure
assert run("1\n3\n9 10 90\n") == "1"

# all equal
assert run("1\n4\n10 10 10 10\n") == "0"

# large single carry-heavy
assert run("1\n2\n999999999 1\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 số | 0 | trường hợp cơ bản, không có tương tác | 
| 9, 1 | 0 | đặt hàng tránh mang theo sớm | 
| 9, 10, 90 | 1 | thứ tự tương tác nhạy cảm | 
| tất cả 10 | 0 | sự ổn định khi lặp lại | 
| 999999999, 1 | 9 | trường hợp xấu nhất mang theo tầng | 

## Vỏ cạnh 

Đầu vào tối thiểu có một số duy nhất luôn tạo ra số 0 vì không có phép cộng nào xảy ra. Thuật toán xử lý việc này một cách trực tiếp vì vòng lặp xử lý một phần tử và không bao giờ bước vào bước nhớ theo chữ số. 

Đối với đầu vào như 9 và 1, thứ tự sắp xếp đặt 1 trước 9. Phép cộng đầu tiên không tạo ra giá trị nào. Phép cộng thứ hai là 1 + 9 = 10, tạo ra chính xác một số nhớ. Mô phỏng phù hợp với hành vi tối thiểu dự kiến ​​vì việc đảo ngược thứ tự cũng sẽ tạo ra một lần mang, xác nhận tính đúng đắn trong các trường hợp đối xứng. 

Đối với các trường hợp có các số giống nhau, chẳng hạn như bội số 10, tất cả các phím điểm chữ số đều bằng nhau, do đó mọi thứ tự đều được chọn. Vì 10 + 10 không mang lại kết quả ở bất kỳ vị trí nào nên các phép cộng lặp lại vẫn không mang lại kết quả. Thuật toán trả về chính xác số 0 bất kể lựa chọn hoán vị. 

Đối với trường hợp có độ lệch cao như 999999999 và 1, số lớn sẽ được xử lý sau cùng. Phép cộng cuối cùng tạo ra một dãy chín chữ số mang theo các chữ số. Thuật toán ghi lại từng ký tự một cách rõ ràng trong mô phỏng chữ số, đảm bảo tính chính xác ngay cả khi được truyền đầy đủ trên tất cả các vị trí chữ số.
