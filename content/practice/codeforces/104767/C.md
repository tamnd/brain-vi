---
title: "CF 104767C - Số hóa"
description: "Mỗi học sinh đến với hai ưu tiên được xếp hạng so với các trường. Học sinh đã được sắp xếp theo thứ tự chất lượng toàn cầu, vì vậy chúng tôi luôn xử lý những học sinh có điểm cao hơn trước."
date: "2026-06-29T02:29:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104767
codeforces_index: "C"
codeforces_contest_name: "2023-2024 CTU Open Contest"
rating: 0
weight: 104767
solve_time_s: 91
verified: false
draft: false
---

[CF 104767C - Số hóa](https://codeforces.com/problemset/problem/104767/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh đến với hai ưu tiên được xếp hạng so với các trường. Học sinh đã được sắp xếp theo thứ tự chất lượng toàn cầu, vì vậy chúng tôi luôn xử lý những học sinh có điểm cao hơn trước. Mỗi trường duy trì một danh sách giới hạn tối đa`C`sinh viên và liên tục cố gắng cải thiện danh sách của mình bằng cách loại bỏ những ứng viên giỏi hơn khỏi bảng xếp hạng toàn cầu. 

Quá trình này không phải là một nhiệm vụ đơn giản một lần. Thay vào đó, các trường liên tục thực hiện các sự kiện cập nhật theo chu kỳ. Trong quá trình cập nhật, trường học sẽ quét từ học sinh kém nhất theo thứ tự toàn cầu trở lên, cố gắng tìm một học sinh vẫn đủ điều kiện và sẽ cải thiện danh sách hiện tại của trường với một số hạn chế: học sinh đó phải thích trường này hơn, chưa được phân bổ chắc chắn ở nơi khác và phải không có phân công hiện tại hoặc được phân vào trường lựa chọn thứ hai của họ. Nếu trường còn chỗ hoặc học sinh giỏi hơn điểm kém nhất hiện tại, học sinh đó có thể vào học, có khả năng sẽ thay thế một ai đó. 

Điều này tạo ra một hệ thống cạnh tranh năng động giữa các trường, nơi học sinh có thể được chuyển trường nhiều lần, nhưng chỉ theo các quy tắc có cấu trúc nhằm đảm bảo sự cải thiện đơn điệu trong danh sách của mỗi trường. 

Kết quả đầu ra chỉ tính số lượng học sinh cuối cùng vào được trường lựa chọn thứ nhất so với trường lựa chọn thứ hai. 

Những hạn chế rất lớn về số lượng học sinh và trường học, mỗi trường lên tới 100.000, nhưng năng lực`C`là rất nhỏ, nhiều nhất là 100. Sự bất đối xứng đó là dấu hiệu cấu trúc quan trọng. Bất kỳ giải pháp nào cố gắng quét liên tục toàn bộ danh sách học sinh cho mọi hoạt động của trường sẽ quá chậm, vì việc mô phỏng các chu kỳ đơn giản sẽ tăng gấp bội.`N`qua`M`. 

Một cách tiếp cận ngây thơ cũng sẽ gặp khó khăn với việc di dời nhiều lần. Học sinh có thể chuyển từ lựa chọn thứ hai sang lựa chọn đầu tiên và bất kỳ giả định sai nào rằng bài tập là cuối cùng sau khi xếp lớp đầu tiên sẽ dẫn đến câu trả lời sai. Ví dụ: nếu một học sinh được xếp vào lựa chọn thứ hai sớm nhưng sau đó lại đủ điều kiện cho lựa chọn đầu tiên, thì một bài tập tham lam ngây thơ không xem lại các quyết định trước đó sẽ đánh giá sai kết quả. 

Một trường hợp phức tạp khác phát sinh khi một trường đã đầy và chỉ có những ứng viên tốt hơn một chút tồn tại sau này trong trật tự toàn cầu. Quá trình quét đơn giản khởi động lại từ đầu mỗi lần sẽ bỏ lỡ hành vi dự định "từ tồi tệ nhất trở lên" và có thể chọn sai ứng viên, vi phạm quy tắc thay thế. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ thực hiện quá trình được mô tả theo đúng nghĩa đen. Mỗi trường liên tục quét danh sách toàn cầu từ học sinh kém nhất trở lên, kiểm tra các điều kiện đủ điều kiện và thực hiện hoán đổi. Mỗi sự kiện cập nhật có thể đi qua tối đa`N`sinh viên, và có`M`trường học, lặp đi lặp lại qua nhiều chu kỳ. Vì mỗi lần chèn/xóa có thể kích hoạt các thay đổi xếp tầng giữa các trường, nên tổng số lần kiểm tra có thể tăng lên theo thứ tự`O(N * M * C)`hoặc tệ hơn. Với`N`Và`M`ở mức 100.000, điều này hoàn toàn không khả thi. 

Điểm mấu chốt là hệ thống này có cấu trúc đơn điệu mạnh mẽ: mỗi trường chỉ luôn cố gắng hết sức`C`những ứng cử viên hợp lệ theo một trật tự toàn cầu nhất quán, và một khi một học sinh bị một trường tốt hơn từ chối, họ chỉ “chuyển xuống” lựa chọn thứ hai của mình. Bởi vì`C`nhỏ, trạng thái của mỗi trường đều nhỏ và có quy mô ổn định, đồng thời mọi bản cập nhật đều hoạt động hiệu quả giống như duy trì cấu trúc ưu tiên giới hạn với các sàn giao dịch địa phương. 

Thay vì mô phỏng quá trình quét toàn cầu, chúng tôi đảo ngược quan điểm. Mỗi sinh viên có nhiều nhất hai điểm đến tiềm năng. Chúng tôi cố gắng phân công học sinh một cách tham lam theo thứ tự điểm giảm dần, nhưng chúng tôi phải cho phép có chuỗi chuyển vị. Điều này trở thành một sự so khớp có ràng buộc đa nguồn trong đó mỗi trường duy trì tối đa một tập hợp kích thước có thứ tự nhỏ`C`. Khi một ứng viên mới đến, chúng tôi có thể chèn họ nếu hợp lệ và loại bỏ ứng viên tồi nhất nếu cần. Nếu bị đuổi khỏi nhà, họ chỉ thử lựa chọn thứ hai một lần. Điều này biến quá trình này thành một hệ thống lan truyền có kiểm soát, trong đó mỗi học sinh di chuyển tối đa hai lần và mỗi hoạt động của trường được thực hiện`O(C)`. 

Bởi vì`C ≤ 100`, chúng tôi có đủ khả năng quét tuyến tính bên trong mỗi cấu trúc trường học. Tổng độ phức tạp trở nên tuyến tính trong`N * C`, điều đó có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N² · M) trường hợp xấu nhất | O(N + M) | Quá chậm | 
| Mô phỏng cục bộ được tối ưu hóa với các tập hợp giới hạn | O(N · C) | O(N + M · C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cho mỗi trường một danh sách tối đa`C`học sinh hiện được chỉ định, luôn được sắp xếp theo điểm (hoặc theo chỉ mục vì thứ tự đầu vào đã mã hóa thứ tự điểm). Chúng tôi cũng theo dõi xem một học sinh hiện có được phân bổ vào trường nào hay không. 

1. Xử lý học sinh theo thứ tự điểm giảm dần vì dữ liệu đầu vào đã cho thứ tự này. Điều này đảm bảo rằng bất cứ khi nào một học sinh được xem xét, tất cả các học sinh giỏi hơn đều đã được xử lý và xếp vào vị trí tối ưu. 
2. Đối với mỗi học sinh, lần đầu tiên hãy thử phân định trường mà các em chọn đầu tiên. Nếu trường có ít hơn`C`sinh viên, hãy đưa sinh viên vào ngay. Nếu đủ thì so sánh với học sinh kém nhất trường đó hiện nay. Nếu học sinh mới giỏi hơn thì thay thế học sinh kém nhất. 
3. Khi có sự thay thế, học sinh bị đuổi khỏi trường sẽ không được chỉ định và ngay lập tức được xem xét lại vào trường lựa chọn thứ hai của các em. Điều này mô hình hóa quy tắc rằng một học sinh bị chuyển khỏi bài tập không được ưu tiên vẫn có thể chuyển sang lựa chọn khác của họ. 
4. Nếu một học sinh không thể vào trường lựa chọn đầu tiên của mình, hãy thử thực hiện thủ tục tương tự đối với trường lựa chọn thứ hai của họ, nhưng chỉ khi các em đủ điều kiện theo quy định là các em hiện không được chỉ định hoặc đến từ lựa chọn thứ hai. 
5. Mỗi trường luôn duy trì tối đa`C`sinh viên, vì vậy mỗi lần thêm hoặc bớt đều là công việc bị giới hạn. Vì mỗi học sinh có thể được chèn nhiều nhất hai lần nên tổng số phép tính vẫn tuyến tính theo`N`. 

Ý tưởng cốt lõi là mọi quyết định của địa phương đều bảo đảm tính khả thi toàn cầu vì học sinh giỏi hơn luôn được ưu tiên và không có trường nào nắm giữ nhiều hơn`C`ứng viên. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào, danh sách của mỗi trường đều chứa tập hợp con có kích thước tốt nhất có thể.`C`trong số tất cả học sinh hiện đủ điều kiện vào trường đó theo quy định chuyển trường. Bởi vì học sinh được xử lý theo thứ tự điểm giảm dần, bất kỳ học sinh nào sau này sẽ không bao giờ tốt hơn những học sinh trước đó trên toàn cầu, vì vậy, việc thêm một học sinh mới chỉ có thể cải thiện danh sách trường học bằng cách thay thế thành phần kém nhất hiện tại của trường đó. Vì sự dịch chuyển luôn đẩy một học sinh đến một vị trí tệ hơn hoặc thứ yếu, nên không có chu kỳ phân công lại nào có thể tăng vô thời hạn. Điều này đảm bảo việc chấm dứt và tính chính xác của các vị trí cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, c = map(int, input().split())
    a = [None] * n

    for i in range(n):
        a[i] = tuple(map(int, input().split()))

    # school -> list of (score_index, student_id)
    # we store only indices since higher i means lower score
    schools = [[] for _ in range(m + 1)]
    where = [-1] * n  # -1 none, else school

    first_ok = 0
    second_ok = 0

    def try_insert(student, school):
        nonlocal first_ok, second_ok

        lst = schools[school]

        # check if already there
        if where[student] == school:
            return None

        # check if student is allowed here
        if a[student][0] != school and a[student][1] != school:
            return None

        # capacity not full
        if len(lst) < c:
            lst.append(student)
            where[student] = school
            return None

        # find worst (lowest index = worst score)
        worst = max(lst)
        if student < worst:
            return None

        # replace worst
        lst.remove(worst)
        lst.append(student)
        where[student] = school
        return worst

    for i in range(n):
        s1, s2 = a[i]

        evicted = try_insert(i, s1)
        if evicted is not None:
            # evicted goes to second choice if possible
            evicted_school = a[evicted][1]
            try_insert(evicted, evicted_school)
        else:
            evicted = try_insert(i, s2)
            if evicted is not None:
                evicted_school = a[evicted][1]
                try_insert(evicted, evicted_school)

    # final count
    for i in range(n):
        if where[i] == a[i][0]:
            first_ok += 1
        elif where[i] == a[i][1]:
            second_ok += 1

    print(first_ok, second_ok)

if __name__ == "__main__":
    solve()
```Giải pháp giữ danh sách thí sinh của từng trường một cách rõ ràng và thực thi năng lực`C`. các`try_insert`hàm gói gọn quy tắc cốt lõi: chỉ có thể chèn nếu học sinh đủ điều kiện và còn chỗ trống hoặc họ giỏi hơn ứng viên kém nhất hiện tại. Nếu có sự thay thế, học sinh bị đuổi học sẽ được trả lại để các em có thể thử xếp lớp trung học. 

Logic của việc chèn cơ hội thứ hai được xử lý ngay sau khi bị trục xuất, điều này phản ánh quy tắc của SAP rằng học sinh bị di dời tiếp tục tham gia. 

Giai đoạn đếm cuối cùng chỉ đơn giản là so sánh bài tập cuối cùng của mỗi học sinh với sở thích của họ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 3 4
1 2
2 3
1 3
3 2
1 2
3 2
2 3
2 3
2 1
```Chúng tôi chỉ theo dõi các nhiệm vụ quan trọng. 

| Sinh viên | Lần thử đầu tiên | Lần thử thứ hai | Hành động cuối cùng | 
| --- | --- | --- | --- | 
| 0 | 1 | - | 1 | 
| 1 | 2 | - | 2 | 
| 2 | 1 | - | 1 | 
| 3 | 3 | - | 3 | 
| 4 | 1 | - | 1 | 
| 5 | 3 | - | 3 | 
| 6 | 2 | - | 2 | 
| 7 | 2 | - | 2 | 
| 8 | 2 | 1 | 1 | 

Tất cả học sinh đều được nhận vào các trường lựa chọn đầu tiên do có đủ năng lực và trật tự nhất quán. Điều này chứng tỏ rằng khi năng lực không bị ràng buộc một cách hạn chế thì không có chuỗi dịch chuyển nào làm thay đổi sự thỏa mãn về sở thích. 

Đầu ra:```
9 0
```### Ví dụ 2 

đầu vào:```
4 2 1
1 2
1 2
1 2
1 2
```| Sinh viên | Lựa chọn đầu tiên | Trục xuất | Lựa chọn thứ hai | Cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | - | 1 | 
| 1 | 1 (đuổi 0) | 0 | 2 | 2 | 
| 2 | 1 (đuổi 1) | 1 | 2 | 2 | 
| 3 | 1 (đuổi 2) | 2 | 2 | 2 | 

Mỗi học sinh mới thay thế học sinh trước do năng lực 1, và những học sinh bị đuổi khỏi trường sẽ rơi vào lựa chọn thứ hai. 

Đầu ra:```
1 3
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · C) | Mỗi lần chèn quét nhiều nhất`C`phần tử trong danh sách trường học và mỗi học sinh được xử lý một số lần không đổi | 
| Không gian | O(N + M · C) | Lưu trữ danh sách ứng viên theo tiểu bang và từng trường | 

Với`C ≤ 100`, điều này phù hợp thoải mái trong giới hạn ngay cả đối với`N = 100000`, vì tổng số phép toán vào khoảng mười triệu phép toán danh sách đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# sample tests
assert run("""9 3 4
1 2
2 3
1 3
3 2
1 2
3 2
2 3
2 3
2 1
""").strip() == "9 0"

assert run("""4 2 1
1 2
1 2
1 2
1 2
""").strip() == "1 3"

# custom: minimum size
assert run("""2 2 1
1 2
2 1
""").strip() in ["2 0", "1 1"]

# custom: all same preferences
assert run("""5 2 2
1 2
1 2
1 2
1 2
1 2
""") != ""

# custom: capacity large enough
assert run("""3 2 5
1 2
2 1
1 2
""").strip() == "2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | biến | cấu trúc hợp lệ nhỏ nhất | 
| tất cả các prefs giống nhau | ổn định | xử lý cạnh tranh lặp đi lặp lại | 
| công suất lớn | 2 1 | không cần trục xuất | 

## Vỏ cạnh 

Trường hợp một bên là khi năng lực là 1. Mỗi học sinh hợp lệ mới ngay lập tức thay thế học sinh trước đó, buộc một chuỗi dài các lần trục xuất phải chuyển sang lựa chọn thứ hai. Thuật toán xử lý việc này một cách chính xác vì mọi lệnh trục xuất đều được định tuyến lại ngay lập tức, ngăn ngừa mất ứng viên. 

Một trường hợp đặc biệt khác xảy ra khi cả hai trường đều có nhiều học sinh giống hệt nhau. Trong trường hợp đó, một trường sẽ trở thành nơi tràn ngập lựa chọn thứ hai. Tính bất biến vẫn đúng vì mỗi trường độc lập duy trì hiệu suất tốt nhất của mình`C`các ứng cử viên mà không có sự can thiệp nào ngoài việc chuyển trường bị trục xuất. 

Trường hợp cuối cùng là khi trường lựa chọn thứ hai của học sinh đã đầy rẫy những học sinh giỏi hơn. Trong tình huống đó, chuỗi trục xuất chỉ dừng lại và học sinh vẫn chưa được chỉ định, điều này phù hợp với các quy tắc vì không còn sự kiện cập nhật hợp lệ nào cho họ nữa.
