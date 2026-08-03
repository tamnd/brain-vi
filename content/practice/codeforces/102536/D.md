---
title: "CF 102536D - Di chuyển để loại bỏ những sai lầm bí mật"
description: "Nhiệm vụ là quyết định xem một người có thể truy cập một phần nội dung hay không dựa trên hai thông tin: tuổi của người đó và danh mục xếp hạng của nội dung. Bản thân tiêu đề không ảnh hưởng đến quyết định, nó chỉ là một phần của định dạng đầu vào."
date: "2026-08-03T21:24:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102536
codeforces_index: "D"
codeforces_contest_name: "2020 UP ACM Algolympics Final Round"
rating: 0
weight: 102536
solve_time_s: 641
verified: true
draft: false
---

[CF 102536D - Di chuyển để xóa các sai lầm bí mật](https://codeforces.com/problemset/problem/102536/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là quyết định xem một người có thể truy cập một phần nội dung hay không dựa trên hai thông tin: tuổi của người đó và danh mục xếp hạng của nội dung. Bản thân tiêu đề không ảnh hưởng đến quyết định, nó chỉ là một phần của định dạng đầu vào. Xếp hạng xác định yêu cầu về độ tuổi tối thiểu, ngoại trừ trường hợp PG đặc biệt khi người trẻ tuổi hơn có thể vào nếu có người lớn chịu trách nhiệm đi cùng. 

Dữ liệu đầu vào chứa một giá trị độ tuổi và một chuỗi xếp hạng ở dòng đầu tiên, theo sau là tiêu đề nội dung ở dòng thứ hai. Đầu ra mô tả quyết định truy cập. Đó có thể là sự chấp thuận thông thường, sự chấp thuận có điều kiện cần có người lớn đi cùng hoặc sự từ chối. 

Độ tuổi có thể lớn tới 10^9, nhưng quyết định chỉ phụ thuộc vào việc so sánh độ tuổi đó với một vài ngưỡng cố định. Độ dài tiêu đề tối đa là 100 ký tự nên việc đọc và lưu trữ nó là chuyện nhỏ. Giải pháp quét tiêu đề hoặc thực hiện bất kỳ tính toán nào tỷ lệ thuận với độ tuổi sẽ không cần thiết. Thuật toán dự định nên sử dụng thời gian không đổi sau khi phân tích cú pháp đầu vào. 

Các trường hợp chính có thể phá vỡ việc thực hiện bất cẩn là độ tuổi ranh giới chính xác. Ví dụ: một người ở độ tuổi chính xác 13 sẽ được phép xem nội dung R-13.```
13 R-13
Example
```Đầu ra đúng là:```
OK
```Một giải pháp sử dụng`age > 13`thay vì`age >= 13`sẽ từ chối trường hợp này một cách không chính xác. 

Một trường hợp quan trọng khác là nội dung PG có một người trẻ. Trẻ một tuổi không bị từ chối ngay vì PG cho phép vào khi có người đi cùng.```
1 PG
Example
```Đầu ra đúng là:```
OK IF ACCOMPANIED
```Một giải pháp coi mọi lứa tuổi dưới 13 là bị cấm sẽ tạo ra kết quả sai. 

Xếp hạng G cũng là một trường hợp đặc biệt vì nó không giới hạn độ tuổi.```
0 G
Example
```Đầu ra đúng là:```
OK
```Bất kỳ triển khai nào áp dụng kiểm tra độ tuổi tối thiểu cho mọi xếp hạng sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra một tập hợp các điều kiện cho mọi xếp hạng và kiểm tra từng điều kiện một. Đây đã là phương pháp bạo lực tự nhiên vì chỉ có năm giá trị xếp hạng có thể có. Công việc của nó là không đổi: nó đọc đầu vào, so sánh tuổi với ngưỡng khi cần thiết và in kết quả tương ứng. 

Không có lực lượng vũ phu chậm hơn có ý nghĩa nào liên quan đến chức danh vì chức danh không có vai trò gì trong việc quyết định. Một cách tiếp cận giả định liên tục kiểm tra mọi giá trị tuổi có thể có sẽ thực hiện tối đa 10^9 lần kiểm tra trong trường hợp lớn nhất, vượt xa thời gian sẵn có. Cấu trúc của bài toán cho chúng ta biết rằng chỉ có mối quan hệ giữa độ tuổi và điểm cắt cố định mới quan trọng. 

Quan sát quan trọng là mọi đánh giá đều có thể được biểu diễn bằng một quy tắc đơn giản. G luôn thành công. PG có một kết quả có điều kiện dành cho độ tuổi dưới 13. R-13, R-16 và R-18 đều là các yêu cầu về độ tuổi tối thiểu. Một khi các quy tắc này được viết rõ ràng, toàn bộ vấn đề sẽ trở thành một số lượng so sánh không đổi. 

Brute-force hoạt động vì số lượng danh mục có thể có là rất nhỏ, nhưng thông tin chi tiết hữu ích nhận ra rằng kích thước đầu vào không yêu cầu bất kỳ tìm kiếm hoặc mô phỏng chung nào. Nhận xét rằng hệ thống xếp hạng là một bảng quyết định cố định cho phép chúng ta rút gọn giải pháp thành một số kiểm tra có điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tuổi của người đó và xếp hạng được yêu cầu. Tiêu đề cũng có thể được sử dụng từ đầu vào nhưng nó không ảnh hưởng đến kết quả. 
2. Nếu xếp hạng là G thì trả về ngay OK vì danh mục này không giới hạn độ tuổi. 
3. Nếu xếp hạng là PG, hãy so sánh độ tuổi với 13. Độ tuổi từ 13 trở lên được phê duyệt bình thường, trong khi những độ tuổi nhỏ hơn sẽ nhận được thông báo phê duyệt kèm theo. 
4. Nếu xếp hạng là R-13, R-16 hoặc R-18, hãy xác định độ tuổi tối thiểu bắt buộc từ số xếp hạng và so sánh với tuổi của người đó. Người đó chỉ được chấp nhận khi độ tuổi ít nhất đạt giá trị yêu cầu. 
5. In chuỗi quyết định khớp với kết quả. 

Lý do điều này có tác dụng là vì các quy tắc xếp hạng tạo thành một phân vùng hoàn chỉnh gồm tất cả các đầu vào có thể có. Mỗi xếp hạng có chính xác một quy tắc và mỗi quy tắc khớp trực tiếp với điều kiện được hệ thống xếp hạng mô tả. 

Tại sao nó hoạt động: 

Đối với bất kỳ đầu vào nào, thuật toán sẽ chọn quy tắc tương ứng với xếp hạng nhất định. Đối với G, quy tắc chấp nhận tất cả mọi người, phù hợp với danh mục không bị hạn chế. Đối với PG, thuật toán chia người thành những người đáp ứng yêu cầu về độ tuổi bình thường và những người cần người đồng hành. Đối với xếp hạng hạn chế, thuật toán chấp nhận chính xác độ tuổi thỏa mãn điều kiện độ tuổi tối thiểu. Vì mọi xếp hạng có thể được xử lý theo quy tắc chính xác của nó nên đầu ra được tạo ra luôn khớp với quyết định truy cập được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

age, rating = input().split()
age = int(age)

if rating == "G":
    print("OK")
elif rating == "PG":
    if age >= 13:
        print("OK")
    else:
        print("OK IF ACCOMPANIED")
else:
    limit = int(rating[2:])
    if age >= limit:
        print("OK")
    else:
        print("ACCESS DENIED")
```Trước tiên, mã sẽ tách độ tuổi khỏi xếp hạng và chuyển đổi độ tuổi thành số nguyên để có thể thực hiện so sánh bằng số. Dòng tiêu đề không cần thiết sau khi phân tích cú pháp vì nó không chứa thông tin nào được sử dụng cho quá trình ra quyết định. 

Nhánh G đứng đầu vì nó hoàn toàn bỏ qua tuổi tác. Nhánh PG có hai kết quả có thể xảy ra, sử dụng`age >= 13`vì vậy trường hợp ranh giới chính xác là 13 được xử lý chính xác. 

Các xếp hạng còn lại đều có cùng định dạng: R theo sau là dấu gạch nối và số. Trích xuất chuỗi con sau hai ký tự đầu tiên sẽ cho ra độ tuổi tối thiểu được yêu cầu. Số nguyên Python có thể xử lý giá trị tuổi tối đa mà không có bất kỳ lo ngại nào về tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
18 R-18
Frozen 3
```| Bước | Tuổi | Đánh giá | Độ tuổi bắt buộc | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 18 | R-18 | 18 | Kiểm tra tuổi >= 18 | 
| 2 | 18 | R-18 | 18 | In đồng ý | 

Con người đã ở chính xác đến ranh giới cần thiết, điều này khẳng định rằng sự bình đẳng phải được chấp nhận. 

Đối với mẫu thứ hai:```
1 R-13
Star Wars: The Fall of Skywalker
```| Bước | Tuổi | Đánh giá | Độ tuổi bắt buộc | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | R-13 | 13 | Kiểm tra tuổi >= 13 | 
| 2 | 1 | R-13 | 13 | In TRUY CẬP TỪ CHỐI | 

Độ tuổi dưới mức yêu cầu tối thiểu nên quyền truy cập bị từ chối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán thực hiện một số phép toán và so sánh chuỗi cố định. | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ. | 

Độ tuổi lớn nhất có thể không làm tăng khối lượng công việc vì nó chỉ được so sánh với những hằng số cố định. Độ dài tiêu đề nhỏ và không ảnh hưởng đến việc tính toán nên giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    age, rating = input().split()
    age = int(age)

    if rating == "G":
        return "OK"
    elif rating == "PG":
        if age >= 13:
            return "OK"
        return "OK IF ACCOMPANIED"
    else:
        limit = int(rating[2:])
        if age >= limit:
            return "OK"
        return "ACCESS DENIED"

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

assert run("18 R-18\nFrozen 3\n") == "OK", "sample 1"
assert run("1 R-13\nStar Wars: The Fall of Skywalker\n") == "ACCESS DENIED", "sample 2"
assert run("13 PG\nAgent Cody Banks\n") == "OK", "sample 3"

assert run("0 G\nAnything\n") == "OK", "minimum age with unrestricted rating"
assert run("12 PG\nMovie\n") == "OK IF ACCOMPANIED", "PG boundary below 13"
assert run("16 R-16\nMovie\n") == "OK", "exact restricted boundary"
assert run("1000000000 R-18\nMovie\n") == "OK", "maximum age value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 G`|`OK`| Xử lý xếp hạng mà không có yêu cầu về độ tuổi. | 
|`12 PG`|`OK IF ACCOMPANIED`| Kiểm tra hành vi PG đặc biệt dưới ngưỡng. | 
|`16 R-16`|`OK`| Kiểm tra sự bình đẳng ở ranh giới xếp hạng bị hạn chế. | 
|`1000000000 R-18`|`OK`| Xác nhận giá trị độ tuổi lớn được xử lý chính xác. | 

## Vỏ cạnh 

Đối với một người ở độ tuổi tối thiểu, thuật toán sử dụng các phép so sánh lớn hơn hoặc bằng.```
13 R-13
Movie
```Xếp hạng không phải là G hoặc PG, do đó mã trích xuất giới hạn là 13 và kiểm tra`13 >= 13`. Điều kiện thành công và câu trả lời là`OK`. 

Đối với xếp hạng PG có trẻ dưới 13 tuổi, thuật toán không từ chối ngay lập tức.```
5 PG
Movie
```Code vào nhánh PG, kiểm tra`5 >= 13`, và thấy nó sai. Vì PG cho phép đệm nên đáp án đúng là`OK IF ACCOMPANIED`. 

Đối với xếp hạng G với một người còn rất trẻ, thuật toán không bao giờ thực hiện so sánh độ tuổi.```
0 G
Movie
```Điều kiện đầu tiên phù hợp và câu trả lời là`OK`. Điều này tránh việc áp dụng sai các hạn chế thuộc về các danh mục khác.
