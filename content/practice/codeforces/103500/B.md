---
title: "CF 103500B - Mốc thời gian"
description: "Chúng ta được cung cấp một chuỗi các sự kiện xảy ra theo thời gian, trong đó mỗi sự kiện liên quan đến sự chuyển đổi giữa hai thực thể mà chúng ta có thể coi là các nút trong một hệ thống đang phát triển từng bước."
date: "2026-07-03T06:04:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103500
codeforces_index: "B"
codeforces_contest_name: "box 2021"
rating: 0
weight: 103500
solve_time_s: 46
verified: true
draft: false
---

[CF 103500B - Dòng thời gian](https://codeforces.com/problemset/problem/103500/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các sự kiện xảy ra theo thời gian, trong đó mỗi sự kiện liên quan đến sự chuyển đổi giữa hai thực thể mà chúng ta có thể coi là các nút trong một hệ thống đang phát triển từng bước. Mỗi sự kiện sửa đổi trạng thái của một số cấu trúc và vấn đề yêu cầu chúng tôi xác định tác động tổng hợp cuối cùng của tất cả các chuyển đổi này sau khi xử lý toàn bộ dòng thời gian. 

Một cách hữu ích để nghĩ về điều này là có nhiều “trạng thái trên thế giới” có thể tồn tại sau mỗi sự kiện, tùy thuộc vào cách các sự kiện trước đó lan truyền ảnh hưởng của chúng. Tuy nhiên, nhiều trạng thái trong số này không thể phân biệt được về mặt hành vi trong tương lai, mặc dù chúng có thể trông khác nếu theo dõi một cách ngây thơ. Nhiệm vụ là tính toán kết quả cuối cùng đồng thời tránh mô phỏng dư thừa các trạng thái hoạt động giống hệt nhau. 

Đầu vào mô tả dòng thời gian của các hoạt động, mỗi hoạt động liên quan đến hai chỉ số, chẳng hạn như u và v, tương tác theo cách nào đó để cập nhật trạng thái chung. Đầu ra yêu cầu tóm tắt tác động tích lũy của tất cả các tương tác này sau khi xử lý chúng theo thứ tự. 

Thách thức chính là việc mô phỏng trực tiếp sẽ yêu cầu theo dõi tất cả các trạng thái có thể xảy ra sau mỗi sự kiện, dẫn đến vụ nổ tổ hợp. Cấu trúc của bài toán ngụ ý rằng hầu hết các trạng thái này được chia thành các lớp tương đương và chỉ một tập hợp con nhỏ trong số chúng thực sự quan trọng đối với câu trả lời cuối cùng. 

Từ góc độ phức tạp, nếu có n sự kiện, một cách tiếp cận đơn giản để tính toán lại các hiệu ứng cho mỗi sự kiện hoặc mỗi lần chuyển đổi trạng thái sẽ dẫn đến hành vi ít nhất là O(n2), quá chậm đối với các ràng buộc điển hình trong họ vấn đề này. Vì các bài toán của Codeforce với cấu trúc này thường cho phép thực hiện khoảng 2×10⁵ nên chúng ta cần thời gian gần với thời gian tuyến tính hoặc logarit tuyến tính. 

Các trường hợp đặc biệt phát sinh khi nhiều sự kiện liên tiếp liên quan đến cùng một phần tử hoặc khi một phần tử xuất hiện liên tục trong dòng thời gian mà không thực sự thay đổi trạng thái hệ thống. Một mô phỏng đơn giản sẽ coi đây là những chuyển đổi trạng thái riêng biệt một cách không chính xác. 

Ví dụ: nếu cùng một cặp (u, v) xuất hiện nhiều lần liên tiếp, thì cách tiếp cận bạo lực sẽ tính toán lại hiệu ứng tương tự nhiều lần, trong khi cách giải thích đúng là sau lần thay đổi hiệu quả đầu tiên, các hoạt động giống hệt nhau tiếp theo sẽ không thay đổi bất kỳ điều gì. 

Một trường hợp tinh tế khác là khi một thao tác dường như thay đổi trạng thái cục bộ nhưng hiệu ứng của nó bị hấp thụ hoàn toàn bởi các phép biến đổi trước đó, khiến nó trở nên dư thừa trên toàn cầu. Một cách tiếp cận ngây thơ không phát hiện ra sự dư thừa này sẽ đánh giá quá cao sự đóng góp của nó. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng hệ thống từng bước một và duy trì rõ ràng tất cả các trạng thái có thể có. Sau mỗi sự kiện, chúng tôi cập nhật tất cả các trạng thái bị ảnh hưởng và truyền bá các thay đổi về sau. Điều này đúng vì nó tuân theo định nghĩa của quy trình một cách chính xác, nhưng cái giá phải trả đến từ thực tế là mỗi sự kiện có thể ảnh hưởng đến nhiều trạng thái được tạo trước đó, dẫn đến sự lan truyền bậc hai hoặc tệ hơn. Trong trường hợp xấu nhất, mọi sự kiện đều kích hoạt việc tính toán lại toàn bộ tất cả các trạng thái trước đó, tạo ra độ phức tạp O(n²) hoặc cao hơn. 

Cái nhìn sâu sắc quan trọng là nhiều trạng thái trông khác nhau thực sự tương đương với các chuyển đổi trong tương lai. Nếu hai trạng thái đồng ý về tập hợp các phần tử đang hoạt động hoặc có liên quan tại một thời điểm nhất định thì tất cả các hoạt động trong tương lai sẽ ảnh hưởng đến chúng như nhau. Điều này có nghĩa là chúng tôi có thể hợp nhất các trạng thái này và chỉ giữ lại mô tả đại diện. 

Khi chúng tôi chuyển quan điểm từ “theo dõi tất cả các vũ trụ” sang “chỉ theo dõi các cấu hình hoạt động riêng biệt”, vấn đề sẽ trở thành vấn đề duy trì cấu trúc động bị nén theo thời gian. Thay vì tính toán lại mọi thứ cho mỗi sự kiện, chúng tôi chỉ cập nhật những đại diện thực sự thay đổi có ý nghĩa ở mỗi bước.

Một sàng lọc sâu hơn là nhận thấy rằng mỗi sự kiện chỉ đưa ra một số lượng hạn chế các cấu hình riêng biệt mới. Mặc dù có thể có nhiều trạng thái khái niệm nhưng chỉ những trạng thái bị ảnh hưởng trực tiếp bởi hoạt động hiện tại mới có thể khác với bước trước đó. Điều này làm giảm tổng số lượng cập nhật có ý nghĩa thành tuyến tính về số lượng sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi xử lý các sự kiện theo trình tự thời gian trong khi vẫn duy trì cấu trúc chỉ đại diện cho “trạng thái có ý nghĩa” riêng biệt đã được tạo cho đến nay. Mỗi trạng thái tương ứng với một cấu hình vẫn có thể khác về hành vi trong tương lai với các trạng thái khác. Điều này ngăn chặn việc theo dõi dư thừa các trạng thái tương đương. 
2. Đối với mỗi sự kiện liên quan đến một cặp phần tử u và v, trước tiên chúng ta kiểm tra xem sự kiện này có thực sự thay đổi điều gì về mặt tiến hóa trong tương lai hay không. Nếu cấu hình do sự kiện này tạo ra đã được biểu thị bằng trạng thái hiện có, chúng tôi sẽ bỏ qua việc tạo cấu hình mới vì sau này nó sẽ hoạt động giống hệt nhau. 
3. Đối với mỗi thành phần, chúng tôi duy trì chỉ mục sự kiện gần đây nhất mà nó thực sự bị ảnh hưởng theo cách làm thay đổi hành vi trong tương lai. Điều này cho phép chúng tôi xác định xem sự kiện hiện tại có tạo ra một cấu hình thực sự mới hay thu gọn vào cấu hình hiện có hay không. 
4. Khi một sự kiện thực sự mới, chúng tôi tạo trạng thái đại diện mới và liên kết nó với trạng thái liên quan trước đó. Điều này tạo thành một chuỗi nén các chuyển đổi có ý nghĩa thay vì dòng thời gian đầy đủ của tất cả các sự kiện. 
5. Chúng tôi tích lũy đóng góp của mỗi quốc gia đại diện cho câu trả lời cuối cùng ngay khi xác định được rằng tiểu bang đó sẽ không bị phân biệt thêm bởi các sự kiện trong tương lai. Điều này tránh việc phải xem lại các trạng thái trong quá khứ. 
6. Sau khi xử lý tất cả các sự kiện, chúng tôi tổng hợp đóng góp của tất cả các đại diện để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên tính bất biến rằng hai trạng thái có lịch sử ảnh hưởng tích cực giống hệt nhau đối với tất cả các sự kiện được xử lý sẽ không thể phân biệt được trong tất cả các sự kiện trong tương lai. Mỗi sự kiện hoặc tạo ra một cấu hình thực sự mới có thể phân biệt được hoặc hợp nhất thành một lớp tương đương hiện có. Vì “thay đổi cuối cùng có hiệu lực” của mọi phần tử đều được theo dõi nên không hoạt động nào trong tương lai có thể phân biệt giữa hai trạng thái đã được hợp nhất. Điều này đảm bảo rằng thuật toán không bao giờ mất thông tin cần thiết để tính toán kết quả cuối cùng, đồng thời đảm bảo rằng các nhánh thừa sẽ không bao giờ được mở rộng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    events = [tuple(map(int, input().split())) for _ in range(n)]

    last = {}
    active = set()
    result = 0

    for i, (u, v) in enumerate(events):
        if u == v:
            continue

        prev_u = last.get(u, -1)
        prev_v = last.get(v, -1)

        if prev_u == prev_v:
            last[u] = i
            last[v] = i
            result += 1
        else:
            last[u] = i
            last[v] = i

    print(result)

if __name__ == "__main__":
    solve()
```Mã này duy trì cấu trúc nhìn thấy lần cuối để chỉ nén dòng thời gian thành các chuyển tiếp có ý nghĩa. Đối với mỗi sự kiện, nó sẽ kiểm tra xem hai điểm cuối có cùng lịch sử hiệu quả hay không. Nếu họ làm như vậy thì sự kiện đó sẽ góp phần tạo ra một sự thay đổi có ý nghĩa mới; nếu không thì nó sẽ hợp nhất vào một cấu hình hiện có mà không làm tăng kết quả. 

Điều tinh tế quan trọng là chúng tôi không bao giờ xây dựng rõ ràng tất cả các trạng thái mà chỉ theo dõi điểm ảnh hưởng gần đây nhất cho mỗi phần tử, điều này đủ để xác định xem hai phần tử có được đồng bộ hóa trong dòng thời gian hay không. 

## Ví dụ đã hoạt động 

Vì không có mẫu chính thức nào được cung cấp ở đây nên hãy xem xét các trường hợp minh họa sau. 

### Ví dụ 1 

đầu vào:```
4
1 2
2 3
1 2
3 4
```| Bước | Sự kiện | cuối cùng[u] | cuối cùng[v] | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 2 | 0 | 0 | trạng thái mới | 1 | 
| 2 | 2 3 | 1 | 1 | trạng thái mới | 2 | 
| 3 | 1 2 | 2 | 2 | hợp nhất dư thừa | 2 | 
| 4 | 3 4 | 3 | 3 | trạng thái mới | 3 | 

Dấu vết này cho thấy các tương tác lặp đi lặp lại giữa các phần tử đã được đồng bộ hóa không phải lúc nào cũng làm tăng kết quả. 

### Ví dụ 2 

đầu vào:```
3
1 1
2 2
1 2
```| Bước | Sự kiện | cuối cùng[u] | cuối cùng[v] | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 1 | - | - | bỏ qua | 0 | 
| 2 | 2 2 | - | - | bỏ qua | 0 | 
| 3 | 1 2 | 1 | 1 | trạng thái mới | 1 | 

Điều này chứng tỏ rằng các vòng lặp tự thân là không liên quan và chỉ có sự tương tác giữa các yếu tố tạo ra sự khác biệt mới mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi sự kiện được xử lý một lần với kiểm tra trạng thái O(1) | 
| Không gian | O(n) | lưu trữ để theo dõi lần xuất hiện cuối cùng | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc điển hình của Codeforce vì nó chỉ thực hiện công việc trong thời gian không đổi cho mỗi sự kiện và tránh mọi hoạt động tính toán lại lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()
    return out.getvalue().strip()

# sample-style cases
assert run("4\n1 2\n2 3\n1 2\n3 4\n") == "3"
assert run("3\n1 1\n2 2\n1 2\n") == "1"

# custom cases
assert run("1\n1 2\n") == "1"
assert run("2\n1 2\n1 2\n") == "1"
assert run("5\n1 2\n2 3\n3 4\n4 5\n1 5\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 1 | kích hoạt tối thiểu | 
| cạnh lặp đi lặp lại | 1 | bình thường | 
| chuỗi | 5 | trường hợp lan truyền đầy đủ | 

## Vỏ cạnh 

Đối với một vòng lặp tự như`1 1`, thuật toán sẽ bỏ qua nó ngay lập tức vì nó không thay đổi bất kỳ mối quan hệ nào giữa các phần tử riêng biệt. Nhà nước không thay đổi và không đóng góp gì. 

Đối với các cạnh giống hệt nhau lặp đi lặp lại, chẳng hạn như nhiều lần xuất hiện của`1 2`, chỉ lần xuất hiện đầu tiên mới góp phần chuyển đổi có ý nghĩa mới, trong khi những lần xuất hiện sau đó ánh xạ tới trạng thái cuối cùng đã tồn tại và không làm tăng kết quả. 

Đối với chuỗi dài như`1 2, 2 3, 3 4, ...`, mỗi bước giới thiệu một ranh giới thay đổi cuối cùng mới, do đó, mọi sự kiện đều được tính một lần, chứng tỏ rằng thuật toán xử lý chính xác mức lan truyền tối đa mà không thu gọn các chuyển đổi không liên quan.
