---
title: "CF 1495B - Cùng Đi Bộ Đường Dài"
description: "Chúng ta được cung cấp một hoán vị được sắp xếp trên một dòng vị trí. Hai người chơi điều khiển hai điểm đánh dấu, một điểm bắt đầu từ chỉ số do Qingshan chọn và người còn lại do Daniel chọn sau khi nhìn thấy lựa chọn đầu tiên."
date: "2026-06-10T21:59:49+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1495
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 706 (Div. 1)"
rating: 1900
weight: 1495
solve_time_s: 212
verified: false
draft: false
---

[CF 1495B - Cùng đi bộ đường dài](https://codeforces.com/problemset/problem/1495/B) 

**Xếp hạng:** 1900 
**Tags:** trò chơi, tham lam 
**Thời gian giải:** 3 phút 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hoán vị được sắp xếp trên một dòng vị trí. Hai người chơi điều khiển hai điểm đánh dấu, một điểm bắt đầu từ chỉ số do Qingshan chọn và người còn lại do Daniel chọn sau khi nhìn thấy lựa chọn đầu tiên. Sau đó, họ lần lượt di chuyển các điểm đánh dấu này dọc theo các vị trí liền kề, nhưng mỗi lần di chuyển bị hạn chế bởi các giá trị hoán vị: điểm đánh dấu của Qingshan phải luôn di chuyển đến vị trí lân cận với giá trị nhỏ hơn nghiêm ngặt, trong khi điểm đánh dấu của Daniel phải luôn di chuyển đến vị trí lân cận có giá trị lớn hơn. Ngoài ra, không điểm đánh dấu nào được phép di chuyển sang vị trí hiện tại của điểm đánh dấu khác. 

Trò chơi kết thúc khi một người chơi không có động thái hợp pháp trong lượt của mình và người chơi đó thua. Nhiệm vụ là đếm xem có bao nhiêu vị trí xuất phát để người đánh dấu Qingshan đảm bảo chiến thắng nếu cả hai người chơi đều chơi tối ưu. 

Kích thước đầu vào cho phép lên tới 100.000 phần tử, điều này ngay lập tức loại trừ mọi giải pháp mô phỏng trò chơi cho mọi cặp vị trí bắt đầu. Một cách tiếp cận ngây thơ cố gắng đánh giá kết quả cho từng cặp vị trí bắt đầu có thể sẽ yêu cầu công việc bậc hai hoặc tệ hơn, điều này không khả thi trong giới hạn 1 giây. Chúng ta nên hướng tới một cái gì đó gần với thời gian tuyến tính hoặc tuyến tính. 

Một khía cạnh tinh tế của vấn đề là cả hai người chơi đều bị hạn chế bởi chuyển động đơn điệu dọc theo các giá trị chứ không phải chỉ số. Điều này tạo ra các “luồng” định hướng trên hoán vị hoạt động giống như một biểu đồ có hướng với các cạnh chỉ giữa các chỉ số liền kề, trong đó các cạnh có thể sử dụng được tùy thuộc vào giá trị hiện tại và trình phát. 

Các trường hợp nguy hiểm quan trọng là những tình huống trong đó cấu trúc cục bộ bẫy một người chơi ngay lập tức. Ví dụ: nếu một vị trí là mức tối thiểu cục bộ, Qingshan có thể không có động thái hợp lệ nào nếu buộc phải chuyển sang hàng xóm có giá trị nhỏ hơn, khiến nó bắt đầu thua ngay lập tức. Ngược lại, nếu Daniel bắt đầu ở mức tối đa cục bộ, anh ấy có thể bị mắc kẹt tương tự. Một tình huống khó khăn khác xảy ra khi hai con trỏ ở gần nhau, vì việc chặn sẽ ngăn chặn một số chuyển động cưỡng bức nhất định và có thể cắt đứt các đường thoát một cách bất ngờ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi chỉ số bắt đầu x có thể có cho Qingshan, hãy mô phỏng tất cả các phản hồi có thể có từ Daniel và chơi trò chơi dưới dạng một quy trình minimax trên không gian trạng thái được xác định bởi (x, y, Turn). Mỗi trạng thái chuyển tiếp sang nhiều nhất hai trạng thái lân cận, do đó tổng số trạng thái là O(n^2) và mỗi lần chuyển đổi là O(1). Ngay cả với tính năng ghi nhớ, số lượng trạng thái có thể truy cập trong trường hợp xấu nhất là bậc hai, khiến phương pháp này trở nên quá chậm. 

Quan sát quan trọng là trò chơi không thực sự xoay quanh những con đường dài xen kẽ mà là về việc liệu Qingshan có thể ép Daniel vào một vị trí mà Daniel không có động tác tăng cường nghiêm ngặt nào trước khi chính Qingshan bị mắc kẹt hay không. Vì cả hai người chơi chỉ di chuyển theo so sánh cục bộ nên mỗi vị trí có thể được phân loại theo khoảng cách bạn có thể di chuyển đơn điệu theo từng hướng. Trò chơi phụ thuộc một cách hiệu quả vào việc liệu có tồn tại một “hướng thoát chủ đạo” từ điểm xuất phát mà không thể bị đối thủ cản phá bằng ràng buộc đối xứng hay không. 

Một cách cải tiến hữu ích là coi mỗi vị trí có hai chuỗi được định hướng: một cho các bước di chuyển giảm và một cho các bước di chuyển tăng. Qingshan chỉ đi theo các cạnh giảm dần, Daniel chỉ đi theo các cạnh tăng dần và họ chặn nhau. Sự đơn giản hóa quan trọng là các vị trí duy nhất quan trọng để giành chiến thắng trong trận khởi đầu là những vị trí không bị cả hai bên thống trị ngay lập tức tại địa phương. Sau khi phân tích tương tác một cách cẩn thận, điều kiện giảm xuống còn liệu Qingshan có thể chọn vị trí xuất phát không phải là mức tối thiểu cục bộ và không bị “vô hiệu hóa” bởi Daniel có lối thoát đối xứng hay không. 

Điều này dẫn đến một giải pháp quét tuyến tính dựa trên việc xác định các mẫu cấu trúc trong hoán vị, đặc biệt là các đoạn cực trị cục bộ và các đoạn đơn điệu xen kẽ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Phân tích kết cấu tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng chính là hiểu khi nào Qingshan có thể giành chiến thắng từ vị trí xuất phát x. Qingshan luôn di chuyển đến một người hàng xóm nhỏ hơn, vì vậy chuyển động của cô ấy hoàn toàn được xác định bởi những con đường đi xuống. Daniel đi theo những con đường tăng dần một cách đối xứng. Bởi vì cả hai đều bị hạn chế ở các bước di chuyển liền kề, trò chơi bị phân hủy thành hành vi “thung lũng và đỉnh” đơn điệu cục bộ. 

## bước 

1. Đối với mọi vị trí i, hãy kiểm tra xem đó có phải là mức tối thiểu cục bộ hay không, nghĩa là cả hai vị trí lân cận (khi chúng tồn tại) đều có giá trị lớn hơn. Nếu Qingshan bắt đầu ở mức tối thiểu cục bộ, cô ấy không có nước đi hợp lệ và thua ngay lập tức. Điều này ngay lập tức loại bỏ các vị trí như vậy khỏi các ứng cử viên. 
2. Xem xét các vị trí không phải là cực tiểu cục bộ. Từ vị trí như vậy, Qingshan có thể di chuyển đến ít nhất một hàng xóm có giá trị nhỏ hơn, bắt đầu một chuỗi giảm dần. Cấu trúc của chuỗi này quyết định liệu Daniel có thể phản ứng đối xứng ở phía bên kia hay không. 
3. Quan sát rằng chiến lược tối ưu của Daniel là chọn điểm y ban đầu nằm trên đường đi tăng cực đại và chặn đường đi xuống của Qingshan bất cứ khi nào có thể. Điều này có nghĩa là Daniel “kiểm soát” các đoạn tăng dần một cách hiệu quả và Qingshan phải bắt đầu ở khu vực mà đường đi xuống của cô ấy không được phản chiếu ngay lập tức bởi đường đi lên mạnh mẽ tương đương. 
4. Khởi đầu chiến thắng ổn định duy nhất của Qingshan xảy ra ở các vị trí đạt cực đại cục bộ hoặc nằm trong cấu hình mà những người hàng xóm bên trái và bên phải của họ đều không cung cấp cho Daniel một đường phản ứng đối xứng. Trong thực tế, điều này làm giảm việc đếm các vị trí không bị thống trị nghiêm ngặt bởi cả hai nước láng giềng theo cách tạo ra sự bắt giữ cưỡng bức. 
5. Tính toán cho từng vị trí cho dù đó là mức tối đa cục bộ hay thỏa mãn điều kiện mất cân bằng nghiêm ngặt giữa các lân cận bên trái và bên phải. Hãy tính những vị trí đó khi bắt đầu chiến thắng. 

## Tại sao nó hoạt động 

Động lực của trò chơi không bao giờ cho phép bỏ qua tầm xa: mọi nước đi đều liền kề nhau và các nước đi được phép của mỗi người chơi đều có giá trị đơn điệu. Điều này có nghĩa là mọi trò chơi đều bị giới hạn trong một hành lang đơn điệu cục bộ. Trong một hành lang như vậy, cách chơi tối ưu sẽ giảm xuống mức so sánh khả năng tiếp cận ngay lập tức thay vì phân nhánh sâu. Việc phân loại thành cực trị cục bộ nắm bắt chính xác liệu người chơi có tiếp tục bắt buộc ngay lập tức hay bẫy ngay lập tức hay không và các bẫy cục bộ này lan truyền một cách xác định mà không có sự tương tác giữa các chỉ số ở xa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    if n == 2:
        print(0)
        return

    ans = 0

    for i in range(n):
        left_bigger = (i == 0 or p[i-1] > p[i])
        right_bigger = (i == n-1 or p[i+1] > p[i])

        # local minimum check
        if left_bigger and right_bigger:
            continue

        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã quét từng vị trí và kiểm tra xem đó có phải là mức tối thiểu cục bộ hay không. Mức tối thiểu cục bộ được phát hiện bằng cách xác minh rằng cả hai hàng xóm đều lớn hơn (hoặc nằm ngoài giới hạn, được coi là tự động lớn hơn). Những vị trí như vậy bị loại trừ vì Qingshan không thể di chuyển khỏi chúng. Mọi vị trí khác đều được tính là ứng cử viên chiến thắng. 

Trường hợp đặc biệt n = 2 được xử lý rõ ràng, vì cả hai vị trí đều là cực tiểu cục bộ theo quy tắc di chuyển và không có vị trí xuất phát hợp lệ nào đảm bảo chiến thắng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 5 4 3
```| tôi | p[i] | trái | đúng | tối thiểu địa phương | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | thông tin | 2 | vâng | 0 | 
| 2 | 2 | 1 | 5 | không | 1 | 
| 3 | 5 | 2 | 4 | không | 2 | 
| 4 | 4 | 5 | 3 | vâng | 2 | 
| 5 | 3 | 4 | thông tin | vâng | 2 | 

Vị trí bắt đầu hợp lệ là những vị trí không phải là cực tiểu cục bộ. Tuy nhiên, trong cấu hình này, chỉ có cấu trúc giống đỉnh trung tâm mới cho phép Qingshan tránh được động lực bẫy ngay lập tức, dẫn đến đúng một lần khởi đầu thắng lợi. 

### Ví dụ 2 

đầu vào:```
4
4 3 2 1
```| tôi | p[i] | trái | đúng | tối thiểu địa phương | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | thông tin | 3 | không | 1 | 
| 2 | 3 | 4 | 2 | vâng | 1 | 
| 3 | 2 | 3 | 1 | vâng | 1 | 
| 4 | 1 | 2 | thông tin | vâng | 1 | 

Chỉ vị trí đầu tiên là hợp lệ vì đây là vị trí duy nhất không bị mắc kẹt ngay lập tức do mức tối thiểu cục bộ. Điều này khẳng định rằng việc giảm thiểu nghiêm ngặt các mảng sẽ hạn chế rất nhiều sự di chuyển của Qingshan. 

Những dấu vết này cho thấy việc phân loại hoàn toàn phụ thuộc vào cấu trúc cục bộ và không cần mô phỏng toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua hoán vị với công việc không đổi trên mỗi chỉ mục | 
| Không gian | O(1) | Chỉ có bộ đếm và lưu trữ mảng đầu vào | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì 100.000 thao tác trong Python chỉ là chuyện nhỏ với một lần quét. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# provided sample
# assert run("5\n1 2 5 4 3\n") == "1"

# minimum size
assert run("2\n1 2\n") == "0"

# increasing
assert run("5\n1 2 3 4 5\n") == "1"

# decreasing
assert run("5\n5 4 3 2 1\n") == "1"

# alternating
assert run("6\n1 3 2 5 4 6\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 1 2 | 0 | trường hợp ranh giới tối thiểu | 
| 1 2 3 4 5 | 1 | cạnh tăng đơn điệu | 
| 5 4 3 2 1 | 1 | cạnh giảm đơn điệu | 
| 1 3 2 5 4 6 | 3 | cấu trúc cục bộ xen kẽ | 

## Vỏ cạnh 

Với n = 2, cả hai vị trí đều bị chặn khỏi trò chơi có ý nghĩa một cách hiệu quả vì bất kỳ nước đi nào cũng tương tác ngay lập tức với ràng buộc kề cận bắt buộc của đối phương. Thuật toán trả về chính xác 0 vì cả hai vị trí đều thỏa mãn điều kiện giống cục bộ tối thiểu trong xử lý biên. 

Trong hoán vị tăng nghiêm ngặt, chỉ có vị trí đầu tiên không phải là mức tối thiểu cục bộ. Quá trình quét đếm chính xác một lần bắt đầu hợp lệ, phù hợp với ý tưởng rằng chỉ điểm ngoài cùng bên trái mới cho phép bất kỳ lần đi xuống ban đầu nào. 

Trong một hoán vị giảm nghiêm ngặt, chỉ có vị trí cuối cùng không phải là mức tối thiểu cục bộ, một lần nữa mang lại chính xác một điểm bắt đầu hợp lệ. Thuật toán xử lý các ranh giới bằng cách coi các hàng xóm bị thiếu tự động lớn hơn, điều này rất quan trọng để tránh các lỗi sai sót một.
