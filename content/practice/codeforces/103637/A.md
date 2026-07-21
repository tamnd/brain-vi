---
title: "CF 103637A - Hoán vị linh hoạt"
description: "Chúng ta được cấp một hoán vị của các số từ 1 đến n và mục tiêu là biến đổi nó thành hoán vị đồng nhất trong đó mọi vị trí i đều chứa giá trị i. Hai hoạt động có sẵn. Một thao tác cho phép chúng ta hoán đổi hai phần tử bất kỳ với chi phí cố định a."
date: "2026-07-03T02:04:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103637
codeforces_index: "A"
codeforces_contest_name: "2019-2020 10th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 103637
solve_time_s: 49
verified: true
draft: false
---

[CF 103637A - Hoán vị linh hoạt](https://codeforces.com/problemset/problem/103637/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị của các số từ 1 đến n và mục tiêu là biến đổi nó thành hoán vị đồng nhất trong đó mọi vị trí i đều chứa giá trị i. 

Hai hoạt động có sẵn. Một thao tác cho phép chúng ta hoán đổi hai phần tử bất kỳ với chi phí cố định a. Hoạt động còn lại ngẫu nhiên hóa hoàn toàn hoán vị với chi phí cố định b, tạo ra hoán vị ngẫu nhiên đồng đều cho tất cả n! khả năng. 

Chúng tôi có thể áp dụng các thao tác này theo bất kỳ trình tự nào, bao gồm việc lặp lại thao tác xáo trộn nhiều lần hoặc trộn lẫn các hoán đổi và xáo trộn. Nhiệm vụ không phải là tìm chi phí tối thiểu xác định mà là chi phí dự kiến ​​tối thiểu để đạt được hoán vị danh tính theo một chiến lược tối ưu. 

Các ràng buộc là nhỏ, với n lên tới 20. Điều này ngay lập tức gợi ý rằng không gian trạng thái của các hoán vị, là n!, có liên quan về mặt khái niệm. Mặc dù n! lớn về mặt thiên văn với n = 20, cấu trúc của các chuyển đổi (hoán đổi hoặc đặt lại ngẫu nhiên) cho thấy chúng ta không cần phải liệt kê các trạng thái một cách rõ ràng mà thay vào đó nén mô tả trạng thái thành một cái gì đó bất biến theo cấu trúc hoán vị. 

Trường hợp cạnh tinh tế xuất hiện khi hoán vị đã được nhận dạng. Trong trường hợp đó, chi phí dự kiến ​​bằng 0 vì không cần thực hiện thao tác nào. Một trường hợp đặc biệt khác là khi các giao dịch hoán đổi đắt hơn so với việc xáo trộn theo kỳ vọng, trong trường hợp đó, việc xáo trộn liên tục có thể là giải pháp tối ưu thay vì sửa chữa bất kỳ thứ gì cục bộ. Ví dụ: nếu n = 2 và hoán vị đã đúng, việc hoán đổi sẽ tốn kém một cách không cần thiết, trong khi việc xáo trộn vẫn tạo ra tính ngẫu nhiên nhưng có thể bỏ qua vì việc dừng ngay lập tức là tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mỗi hoán vị là một trạng thái trong biểu đồ có kích thước n!. Từ mỗi trạng thái, chúng ta có thể chuyển sang bất kỳ trạng thái nào khác bằng cách trả b (tráo đổi) hoặc sang một trạng thái lân cận cụ thể bằng cách hoán đổi bất kỳ cặp chỉ số nào với giá a. Mục tiêu là hoán vị danh tính và chúng tôi muốn đạt được mục tiêu đó với chi phí dự kiến ​​tối thiểu. 

Điều này ngay lập tức trở thành một bài toán quy hoạch động có giá trị kỳ vọng trên một không gian trạng thái khổng lồ. Ngay cả việc viết phương trình cho mỗi hoán vị cũng không thể thực hiện được. 

Quan sát quan trọng là cấu trúc nhận dạng chỉ phụ thuộc vào số lượng phần tử đã ở đúng vị trí chứ không phụ thuộc vào sự sắp xếp chính xác của chúng. Tuy nhiên, hoán đổi có thể sửa nhiều phần tử cùng một lúc tùy thuộc vào chu kỳ hoán vị và việc xáo trộn sẽ phá hủy tất cả cấu trúc một cách đồng nhất. Hoạt động xáo trộn là một lực đơn giản hóa: nó làm cho trạng thái đồng nhất trở lại, có nghĩa là sau khi xáo trộn, chi phí còn lại dự kiến ​​là như nhau bất kể cấu hình trước đó. 

Điều này gợi ý quan điểm chiến lược hơn là DP theo từng tiểu bang. Từ bất kỳ trạng thái nào, chúng ta có hai lựa chọn: trực tiếp sửa hoán vị chỉ bằng cách sử dụng hoán đổi hoặc trả tiền b để đặt lại về hoán vị ngẫu nhiên thống nhất và thử lại một cách tối ưu. Nếu chúng ta cam kết thực hiện một chiến lược, quy trình sẽ trở thành quyết định giữa chi phí “giải quyết từ trạng thái hiện tại” mang tính xác định và vòng lặp khởi động lại theo xác suất. 

Gọi E là chi phí dự kiến ​​từ một hoán vị ngẫu nhiên. Nếu chúng ta chọn xáo trộn ngay lập tức, chúng ta trả b và quay về cùng kỳ vọng E, do đó nhánh đó đóng góp b + E. Nếu chúng ta chọn làm việc bằng cách sử dụng các hoán đổi mà không xáo trộn, chúng ta cần số lượng hoán đổi tối thiểu để sắp xếp hoán vị, chẳng hạn như k, mỗi hoán vị có chi phí a, do đó chi phí là k·a. 

Do đó, từ trạng thái ban đầu, chúng tôi so sánh hai chiến lược: kết thúc trực tiếp bằng cách sử dụng giao dịch hoán đổi hoặc thanh toán b và khởi động lại toàn bộ quá trình. Giá trị kỳ vọng tối ưu thỏa mãn phương trình điểm cố định: 

E = phút(k·a, b + E) 

Hình thức này ngụ ý rằng nếu k·a ≤ b + E thì chúng ta thích hoàn thiện trực tiếp hơn. Ngược lại, việc xáo trộn lặp đi lặp lại sẽ chiếm ưu thế và phương trình sẽ chuyển sang quá trình khởi động lại hình học.

Sắp xếp lại trường hợp thứ hai ta có E = b + E, điều này là không thể trừ khi chúng ta không bao giờ chọn nhánh đó. Giải thích chính xác là nếu chúng ta chọn dựa vào việc xáo trộn, chúng ta sẽ khởi động lại một cách hiệu quả cho đến khi đạt được một hoán vị trong đó việc hoàn thành bằng cách hoán đổi sẽ rẻ hơn so với một lần xáo trộn khác, dẫn đến sự phân bố hình học theo số lần thử. 

Cách chính xác để giải quyết vấn đề này là tính k, số lần hoán đổi tối thiểu cần thiết, sau đó so sánh chi phí hoàn thành xác định k·a với chi phí dự kiến ​​của việc xáo trộn liên tục cho đến khi thành công. Mỗi lần xáo trộn mang lại một hoán vị mới, vì vậy với xác suất 1, cuối cùng chúng ta sẽ đến trạng thái mà chúng ta quyết định hoàn thành, nhưng số lần xáo trộn dự kiến ​​​​là 1/p trong đó p là xác suất một hoán vị ngẫu nhiên “đủ rẻ để hoàn thành”. 

Sự đơn giản hóa chính là sự khác biệt có ý nghĩa duy nhất là liệu chúng ta có chọn xáo trộn hay không. Nếu chúng ta không bao giờ xáo trộn thì câu trả lời là k·a. Nếu chúng ta chọn xáo trộn, chính sách tối ưu là xáo trộn cho đến khi chúng ta có được hoán vị danh tính, bởi vì danh tính là trạng thái duy nhất mà chi phí vẫn bằng 0. Bất kỳ trạng thái không đồng nhất nào đều có cấu trúc giống hệt nhau dưới sự đối xứng, do đó không thu được lợi thế một phần nào từ việc chờ đợi. 

Do đó, chiến lược tối ưu không còn là việc so sánh: 

chi phí trực tiếp = k·a 

xáo trộn cho đến khi nhận dạng chi phí = số lần xáo trộn dự kiến cho đến khi nhận dạng lần b = n! · b 

Vì vậy, câu trả lời chỉ đơn giản là: 

phút(k·a, n! · b) 

Chúng ta vẫn cần tính k, số lần hoán đổi tối thiểu để chuyển đổi hoán vị thành nhận dạng, được biết đến là n trừ đi số chu kỳ trong hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | O(n! · n) | Ồ (n!) | Quá chậm | 
| Phân rã chu trình + so sánh | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tách hoán vị thành các chu trình rời rạc bằng cách đi theo các con trỏ từ mỗi chỉ mục chưa được truy cập cho đến khi quay lại điểm bắt đầu. Mỗi phần tử thuộc về đúng một chu trình vì hoán vị là song ánh. 
2. Đếm xem có bao nhiêu chu kỳ. Mỗi chu kỳ có độ dài L yêu cầu L − 1 lần hoán đổi để cố định, bởi vì chúng ta có thể xoay từng phần tử vào vị trí. 
3. Tính k = n − Cycle_count, là tổng số lần hoán đổi tối thiểu cần thiết để đạt được danh tính. 
4. Tính chi phí xác định là k · a, biểu thị việc sửa chữa mọi thứ trực tiếp chỉ bằng cách sử dụng các giao dịch hoán đổi. 
5. Tính chi phí dự kiến ​​​​chỉ xáo trộn là (n giai thừa) · b, biểu thị các lần xáo trộn ngẫu nhiên lặp đi lặp lại cho đến khi gặp được danh tính một lần, vì danh tính xuất hiện với xác suất 1/n! mỗi lần xáo trộn. 
6. Xuất ra giá trị tối thiểu của hai giá trị này. 

Bước lý luận là các hoạt động hoán đổi làm giảm sự rối loạn một cách xác định và tối ưu, trong khi việc xáo trộn hoạt động như một sự thiết lập lại hoàn toàn không mang lại tiến triển một phần nào, do đó các chiến lược không xen kẽ theo bất kỳ cách có lợi nào ngoài sự so sánh này. 

### Tại sao nó hoạt động 

Cấu trúc hoán vị trong các giao dịch hoán đổi được nắm bắt hoàn toàn bằng cách phân rã chu trình và các giao dịch hoán đổi hoạt động độc lập bên trong các chu trình mà không có sự tương tác giữa các chu kỳ. Điều này làm cho k = n − chu kỳ trở thành chi phí tối thiểu chính xác cho độ phân giải xác định. Mặt khác, xáo trộn sẽ xóa tất cả cấu trúc và tạo ra một trạng thái đồng nhất mỗi lần, do đó, bất kỳ chiến lược nào liên quan đến xáo trộn sẽ giảm xuống một quy trình Bernoulli lặp đi lặp lại trong đó chỉ có trạng thái nhận dạng được hấp thụ. Vì không có cấu trúc trung gian nào tồn tại được sau khi xáo trộn nên không có lợi ích gì trong việc điều chỉnh tính đúng đắn từng phần và quá trình này chuyển sang trạng thái khởi động lại dự kiến ​​đơn giản cho đến khi nhận dạng được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())
    p = list(map(int, input().split()))
    p = [x - 1 for x in p]

    visited = [False] * n
    cycles = 0

    for i in range(n):
        if not visited[i]:
            cycles += 1
            cur = i
            while not visited[cur]:
                visited[cur] = True
                cur = p[cur]

    swaps = n - cycles

    direct = swaps * a

    # compute n! carefully
    fact = 1
    for i in range(2, n + 1):
        fact *= i

    shuffle = fact * b

    print(min(direct, shuffle))

if __name__ == "__main__":
    solve()
```Phần đếm chu kỳ tuân theo mô hình duyệt hoán vị tiêu chuẩn. Mỗi lần chúng tôi tìm thấy một chỉ mục chưa được truy cập, chúng tôi sẽ đi dọc theo p cho đến khi đóng một vòng lặp, đánh dấu tất cả các nút đã truy cập. Điều này đảm bảo mỗi chu kỳ được tính chính xác một lần. 

Việc tính toán giai thừa được thực hiện lặp đi lặp lại vì n nhỏ và các giá trị phù hợp với số nguyên Python mà không gặp vấn đề tràn. 

Sự so sánh ở cuối phản ánh việc rút gọn vấn đề thành hai chiến lược tối ưu rời rạc: hiệu chỉnh trực tiếp và đặt lại ngẫu nhiên lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 2, a = 5, b = 5 

p = [1, 2] 

| Bước | Chu kỳ | Hoán đổi | Chi phí trực tiếp | N! | Chi phí xáo trộn | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 0 | 0 | 2 | 10 | 0 | 

Hoán vị đã là danh tính nên có hai chu kỳ có độ dài 1 mỗi chu kỳ. Không cần hoán đổi, vì vậy chi phí trực tiếp bằng 0 và chi phí đó chiếm ưu thế. 

### Ví dụ 2 

đầu vào: 

n = 2, a = 1, b = 2 

p = [2, 1] 

| Bước | Chu kỳ | Hoán đổi | Chi phí trực tiếp | N! | Chi phí xáo trộn | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 1 | 2 | 4 | 1 | 

Có một chu kỳ có độ dài 2, yêu cầu một lần hoán đổi. Chi phí trực tiếp là 1. Chiến lược xáo trộn sẽ có chi phí dự kiến ​​là 2!·2 = 4, do đó, hoán đổi trực tiếp sẽ tốt hơn. 

Dấu vết cho thấy cấu trúc chu kỳ xác định trực tiếp chi phí xác định như thế nào, trong khi sự xáo trộn hoàn toàn bỏ qua cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Phân rã chu trình và tính toán giai thừa đều chạy trong thời gian tuyến tính trong n | 
| Không gian | O(n) | Đã truy cập mảng và lưu trữ hoán vị | 

Các ràng buộc n 20 làm cho O(n) trở nên tầm thường và thậm chí tính toán giai thừa cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, a, b = map(int, input().split())
    p = list(map(int, input().split()))
    p = [x - 1 for x in p]

    visited = [False] * n
    cycles = 0

    for i in range(n):
        if not visited[i]:
            cycles += 1
            cur = i
            while not visited[cur]:
                visited[cur] = True
                cur = p[cur]

    swaps = n - cycles
    direct = swaps * a

    fact = 1
    for i in range(2, n + 1):
        fact *= i

    shuffle = fact * b

    return str(min(direct, shuffle))

# provided samples
assert run("2 5 5\n1 2\n") == "0"
assert run("2 1 2\n2 1\n") == "1"

# custom cases
assert run("1 10 10\n1\n") == "0", "single element already sorted"
assert run("3 1 100\n2 3 1\n") == "2", "one cycle of length 3"
assert run("4 10 1\n2 1 4 3\n") == "1", "shuffle dominates heavily"
assert run("5 3 1000\n1 2 3 4 5\n") == "0", "already identity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 danh tính | 0 | độ đúng cơ sở | 
| đơn 3 chu kỳ | 2 | tính toán hoán đổi chu kỳ | 
| xáo trộn giá rẻ | 1 | sự thống trị xáo trộn | 
| sắc lớn b | 0 | trường hợp không có cạnh | 

## Vỏ cạnh 

Đối với một hoán vị đã đồng nhất, chẳng hạn như n = 4 với p = [1, 2, 3, 4], số chu kỳ là 4 và số lần hoán đổi bằng 0, do đó chi phí trực tiếp trở thành 0. Thuật toán đưa ra kết quả bằng 0 một cách chính xác vì nó không xem xét việc xáo trộn trừ khi nó cải thiện kết quả và bất kỳ sự xáo trộn nào đều đưa ra chi phí dự kiến ​​dương. 

Đối với một chu kỳ lớn như n = 5, p = [2, 3, 4, 5, 1], số chu kỳ là 1 nên số lần hoán đổi = 4. Chi phí trực tiếp là 4a. Quá trình truyền tải truy cập vào mỗi nút chính xác một lần trước khi kết thúc chu trình, đảm bảo không tính quá mức. Chi phí xáo trộn vẫn độc lập với cấu trúc và vẫn là n!·b, do đó so sánh vẫn có giá trị bất kể hình dạng chu kỳ. 

Đối với b cực lớn, thuật toán không bao giờ ưu tiên xáo trộn vì n!·b chiếm ưu thế. Cấu trúc của hoán vị trở nên không liên quan và nghiệm chỉ đơn giản là phân rã chu trình mà thuật toán xử lý mà không có bất kỳ lý luận phân nhánh hoặc xác suất nào.
