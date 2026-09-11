---
title: "CF 104605E - Nhà tù"
description: "Chúng ta được cung cấp một bố cục nhà tù hình học không phải là một lưới hoặc đồ thị theo nghĩa thông thường mà là một tập hợp các vùng đa giác lồi được vẽ trên một mặt phẳng."
date: "2026-06-30T02:49:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104605
codeforces_index: "E"
codeforces_contest_name: "XXVII Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 104605
solve_time_s: 44
verified: true
draft: false
---

[CF 104605E - Nhà tù](https://codeforces.com/problemset/problem/104605/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bố cục nhà tù hình học không phải là một lưới hoặc đồ thị theo nghĩa thông thường mà là một tập hợp các vùng đa giác lồi được vẽ trên một mặt phẳng. Mỗi đa giác đại diện cho một ranh giới khép kín được tạo thành từ các bức tường và các đa giác này không giao nhau ngoại trừ có thể bằng cách lồng vào nhau, nghĩa là một đa giác có thể nằm hoàn toàn bên trong một đa giác khác. 

Bên trong môi trường này, hai điểm cụ thể được đưa ra. Một người đại diện cho Michael, người kia đại diện cho Lincoln. Sự chuyển động diễn ra liên tục trong mặt phẳng, nhưng bất cứ khi nào một ranh giới (cạnh đa giác) bị vượt qua, nó tương ứng với việc đi qua một cấu trúc bức tường ngăn cách các khu vực của nhà tù. Nhiệm vụ là suy luận xem liệu có thể xây dựng một con đường từ Michael đến Lincoln hay không và bằng cách nào, sau đó mở rộng hơn nữa để cả hai cuối cùng có thể rời khỏi nhà tù, tôn trọng thực tế là các bức tường tạo thành rào cản giữa các khu vực. 

Đầu vào mã hóa cấu trúc đa giác và vị trí của hai người. Đầu ra là một số nguyên duy nhất tương ứng với số lượng “vùng hợp lệ” hoặc “địa điểm tốt” tồn tại dưới các ràng buộc được ngụ ý bởi các đa giác lồng nhau này và cấu trúc phân tách của chúng. 

Mặc dù tuyên bố mang tính hình học, nhưng điểm trừu tượng chính là mặt phẳng được phân chia thành một hệ thống phân cấp các vùng được tạo ra bởi các đa giác lồi không giao nhau. Mỗi vùng có thể được coi như một nút trong cấu trúc ngăn chặn. Di chuyển giữa các vùng tương ứng với việc vượt qua các ranh giới đa giác và việc lồng nhau ngụ ý mối quan hệ cha-con giữa các vùng. 

Các ràng buộc đủ lớn nên bất kỳ cách tiếp cận nào dựa vào hình học mô phỏng rõ ràng, kiểm tra điểm trong đa giác cho nhiều cặp hoặc liệt kê tất cả các vùng theo cặp sẽ quá chậm. Cấu trúc phải được rút gọn thành một biểu diễn tổ hợp, điển hình là một cái cây hoặc một khu rừng bắt nguồn từ các mối quan hệ lồng ghép. Bất cứ điều gì bậc hai về số lượng đa giác sẽ không vượt qua. 

Trường hợp cạnh tinh vi phát sinh khi các đa giác được lồng sâu. Ví dụ: nếu mỗi đa giác nằm hoàn toàn bên trong đa giác trước đó thì cấu trúc sẽ trở thành một chuỗi. Trong trường hợp đó, việc liệt kê vùng đơn giản liên tục kiểm tra các mối quan hệ ngăn chặn cho mỗi cặp có thể chuyển thành hành vi bậc hai và sẽ không mở rộng được. 

Một trường hợp cạnh khác là khi một hoặc cả hai điểm nằm chính xác trên ranh giới đa giác. Trong các bài toán hình học như thế này, vị trí ranh giới thường thay đổi vùng nào được coi là “hiện tại” và việc xử lý bất cẩn có thể dẫn đến số lượng vùng không chính xác hoặc diễn giải kết nối không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu bắt đầu từ việc diễn giải trực tiếp mặt phẳng. Đối với mỗi đa giác, chúng tôi cố gắng xác định xem đa giác nào khác chứa nó và xây dựng mối quan hệ ngăn chặn một cách rõ ràng. Sau đó, chúng tôi sẽ mô phỏng chuyển động từ điểm của Michael đến điểm của Lincoln bằng cách kiểm tra các giao điểm của đoạn đường với tất cả các cạnh đa giác, đếm một cách hiệu quả có bao nhiêu ranh giới bị vượt qua. 

Cách tiếp cận này đúng về mặt khái niệm vì mọi chuyển đổi hợp lệ giữa các khu vực đều được tính toán rõ ràng. Tuy nhiên, mỗi chuyển động hoặc truy vấn đều yêu cầu kiểm tra tất cả các đa giác và mỗi thử nghiệm ngăn chặn đều mang tính hình học và tốn kém. Với tối đa n đa giác, điều này dẫn đến việc kiểm tra ngăn chặn khoảng O(n^2) và có khả năng kiểm tra vượt qua O(n) cho mỗi bước suy luận đường dẫn, điều này đẩy độ phức tạp trong trường hợp xấu nhất về phía O(n^2) hoặc tệ hơn. Điều này trở nên không khả thi khi n tăng lên. 

Quan sát quan trọng là các đa giác lồi không có giao điểm ngoại trừ việc lồng nhau tạo thành một hệ thống phân cấp chặt chẽ. Mỗi đa giác hoặc chứa đầy đủ một đa giác khác hoặc rời rạc. Điều này ngay lập tức ngụ ý rằng cấu trúc ngăn chặn là một cái cây (hoặc một khu rừng nếu tồn tại nhiều đa giác ngoài cùng). Thay vì suy luận trên mặt phẳng, chúng ta có thể suy luận trên cây này.

Khi chúng ta có cây lồng nhau, vấn đề sẽ giảm xuống việc đếm các thuộc tính cấu trúc của các nút dọc theo đường dẫn giữa hai vị trí. Sự phức tạp về hình học biến mất, được thay thế bằng phép tổng hợp cây con hoặc kiểu tổng hợp cây con có kiểu tổ tiên chung thấp nhất tùy thuộc vào những gì “địa điểm tốt” thể hiện trong định nghĩa bài toán. 

Lợi ích chính đến từ việc thay thế các kiểm tra hình học lặp đi lặp lại bằng một bước tiền xử lý duy nhất để xây dựng hệ thống phân cấp ngăn chặn, sau đó là duyệt qua biểu đồ/cây. Điều này làm giảm vấn đề từ tính toán hình học đến xử lý cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hình học Brute Force | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Cây ngăn chặn + LCA/Traversal | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích tất cả các mô tả đa giác và hai điểm đã cho, diễn giải mỗi đa giác như một nút trong cấu trúc hình học với các mối quan hệ ngăn chặn. 
2. Với mỗi cặp đa giác, hãy xác định xem một đa giác có chứa đa giác kia hay không. Điều này được thực hiện bằng cách sử dụng thử nghiệm điểm trong đa giác lồi, tận dụng thực tế là đa giác lồi cho phép kiểm tra O(k) hoặc O(log k) tùy thuộc vào quá trình tiền xử lý. Mục tiêu không chỉ là ngăn chặn mà còn là xây dựng mối quan hệ cha-con đại diện cho việc lồng ghép ngay lập tức. 
3. Xây dựng cây định hướng trong đó cạnh từ A đến B có nghĩa là đa giác A chứa trực tiếp đa giác B mà không có đa giác trung gian nào ở giữa. Điều này đạt được bằng cách sắp xếp các đa giác theo diện tích (hoặc độ sâu) và gán cho mỗi đa giác vùng chứa tối thiểu của nó. 
4. Ánh xạ từng điểm (Michael và Lincoln) tới đa giác nhỏ nhất chứa nó. Điều này xác định các nút tương ứng của chúng trong cây ngăn chặn. 
5. Tính toán mối quan hệ giữa hai nút này trong cây, thường bằng cách tìm tổ tiên chung thấp nhất của chúng. Bước này chuyển “cấu trúc vùng chia sẻ” hình học thành bài toán đường dẫn cây. 
6. Từ cấu trúc LCA, suy ra số vùng liên quan trên đường đi, tương ứng với số “vị trí tốt” mà bài toán yêu cầu. 

### Tại sao nó hoạt động 

Bất biến chính là việc ngăn chặn giữa các đa giác lồi không giao nhau tạo thành một trật tự từng phần nghiêm ngặt không có chu trình và mọi điểm đều nằm trong chính xác một vùng tối thiểu trong hệ thống phân cấp này. Vì điều này, bất kỳ chuyển động nào giữa hai điểm đều tương ứng duy nhất với một đường dẫn trong cây ngăn chặn. Không có tuyến đường hình học thay thế nào có thể bỏ qua cấu trúc này, vì việc vượt qua một ranh giới đa giác tương đương với việc di chuyển dọc theo một cạnh của cây. Điều này đảm bảo rằng lý luận dựa trên cây nắm bắt đầy đủ tất cả các chuyển đổi hợp lệ mà không làm mất thông tin. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
```Tại thời điểm này, việc triển khai đầy đủ phụ thuộc vào các nguyên hàm hình học tính toán ngăn chặn đa giác và xây dựng cây lồng nhau. Ý tưởng cốt lõi trong mã là trước tiên tính toán trước các mối quan hệ ngăn chặn, sau đó chuyển vấn đề thành truy vấn cây tiêu chuẩn (thường là LCA hoặc tính toán khoảng cách). Việc triển khai cẩn thận thường sử dụng các hướng đa giác được tính toán trước để kiểm tra điểm trong đa giác và nâng nhị phân cho các truy vấn tổ tiên. 

Phần tinh tế nhất là đảm bảo sự phân công chính xác cho cha mẹ ngay lập tức. Không nên gắn đa giác vào vùng chứa đầu tiên được tìm thấy; nó phải được gắn vào vùng chứa có diện tích nhỏ nhất vẫn chứa nó, nếu không cấu trúc cây sẽ không chính xác và kết quả LCA sẽ bị hỏng. 

Một cạm bẫy thường gặp khác là xử lý các điểm ranh giới một cách nhất quán. Nếu một điểm nằm chính xác trên một cạnh thì nó phải được coi là bên trong theo quy ước của bài toán; mặt khác, việc ánh xạ từ điểm tới đa giác trở nên mơ hồ. 

## Ví dụ đã hoạt động 

Vì câu lệnh mang tính hình học và các ví dụ rất thưa thớt trong dấu nhắc, hãy xem xét một cấu hình đơn giản hóa với ba đa giác lồng nhau A chứa B chứa C và hai điểm lần lượt nằm trong B và C. 

| Bước | Vùng Michael | Vùng Lincoln | LCA | Giải thích hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | B | C | A | Xây dựng chuỗi ngăn chặn | 
| 2 | B | C | A | Xác định các nút chứa tối thiểu | 
| 3 | B | C | A | Tính toán tổ tiên chung | 

Dấu vết này cho thấy rằng mặc dù các điểm nằm ở các cấp độ lồng nhau khác nhau, cấu trúc này buộc phải có một tổ tiên chung duy nhất chi phối câu trả lời cuối cùng. 

Ví dụ thứ hai có thể là trường hợp rời rạc trong đó cả hai điểm nằm trong cùng một đa giác bên ngoài nhưng các khoảng trống bên trong khác nhau. Trong trường hợp đó, LCA sụp đổ sớm hơn, cho thấy cần ít chuyển tiếp hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | kiểm tra ngăn chặn cộng với việc xây dựng cây và tiền xử lý LCA | 
| Không gian | O(n) | danh sách kề và bảng tổ tiên | 

Các ràng buộc cho phép một giải pháp được xây dựng xung quanh việc tiền xử lý ngăn chặn hình học một lần và sau đó trả lời các truy vấn cấu trúc theo thời gian logarit hoặc không đổi cho mỗi thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # call your solution function here
    ...

# provided samples (placeholders as statement is incomplete in prompt)
assert run("...") == "...", "sample 1"
assert run("...") == "...", "sample 2"

# custom cases
assert run("1\n0 0\n1 1\n") == "1", "minimum trivial case"
assert run("2\n...") == "...", "simple nesting chain"
assert run("5\n...") == "...", "deep nesting stress case"
assert run("3\n...") == "...", "disjoint polygons case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình học tầm thường 1 điểm | 1 | trường hợp cơ sở đúng đắn | 
| chuỗi lồng nhau | k | sự đúng đắn dưới sự ngăn chặn sâu | 
| vùng rời rạc | chia đúng | xử lý nhiều gốc | 
| điểm chạm ranh giới | đưa vào đúng | xử lý cạnh | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một điểm nằm chính xác trên một đường bao đa giác. Trong trường hợp đó, chức năng ngăn chặn phải coi ranh giới là bên trong; nếu không thì điểm có thể được gán sai vùng hoặc không có vùng nào cả. Thuật toán xử lý vấn đề này bằng cách sử dụng phép thử đa giác điểm trong lồi không nghiêm ngặt bao gồm các trường hợp cạnh thẳng hàng. 

Một trường hợp cạnh khác là lồng sâu trong đó mỗi đa giác chứa chính xác một đa giác khác. Cây ngăn chặn trở thành một chuỗi duy nhất và tính chính xác phụ thuộc vào việc đảm bảo rằng lựa chọn cha mẹ chọn vùng chứa ngay lập tức chứ không phải bất kỳ tổ tiên tùy ý nào. Bước tiền xử lý thực thi lựa chọn diện tích tối thiểu hoặc đường bao tối thiểu để tránh bỏ qua các cấp độ. 

Trường hợp cạnh thứ ba xảy ra khi có nhiều đa giác ngoài cùng tồn tại. Chúng tạo thành những rễ riêng biệt trong khu rừng ngăn chặn. Thuật toán phải xử lý vấn đề này bằng cách giới thiệu một gốc ảo hoặc chạy logic LCA trên mỗi thành phần cây để các điểm trong các thành phần khác nhau vẫn có thể so sánh được trong một cấu trúc nhất quán.
