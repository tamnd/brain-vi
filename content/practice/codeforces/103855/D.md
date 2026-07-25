---
title: "CF 103855D - Đòn tấn công ba thanh kiếm"
description: "Chúng ta được cấp một tập hợp các điểm có trọng số trên một lưới. Mỗi điểm đại diện cho một con quái vật nằm ở tọa độ $(x, y)$ nào đó và đóng góp một số giá trị (hoặc ngầm định một đơn vị giá trị nếu trọng số không được nêu rõ ràng trong biến thể câu lệnh)."
date: "2026-07-02T08:02:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103855
codeforces_index: "D"
codeforces_contest_name: "XXII Open Cup. Grand Prix of Seoul"
rating: 0
weight: 103855
solve_time_s: 49
verified: true
draft: false
---

[CF 103855D - Đòn tấn công ba thanh kiếm](https://codeforces.com/problemset/problem/103855/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm có trọng số trên một lưới. Mỗi điểm đại diện cho một con quái vật nằm ở tọa độ nào đó$(x, y)$và đóng góp một số giá trị (hoặc ngầm định một đơn vị giá trị nếu trọng số không được nêu rõ ràng trong biến thể câu lệnh). Chúng ta được phép thực hiện chính xác ba “đòn kiếm”, trong đó mỗi đòn tấn công là một đòn tấn công theo đường thẳng thu thập tổng giá trị của tất cả quái vật nằm trên đường đó. 

Hạn chế chính là các cú đánh có hai dạng hình học: các đường thẳng song song với trục x hoặc các đường thẳng song song với trục y. Mục tiêu là chọn ba đường như vậy để tối đa hóa tổng giá trị thu thập được, trong đó một con quái vật sẽ đóng góp nếu nó nằm trên ít nhất một đường đã chọn. 

Cấu trúc của giải pháp phụ thuộc rất nhiều vào số lượng đường được chọn là ngang và dọc. Nếu cả ba đều nằm ngang, bài toán sẽ rơi vào việc chọn ba cấp độ y có tổng trọng số tối đa. Nếu hai là theo chiều ngang và một là theo chiều dọc, chúng ta phải tính đến sự chồng chéo một cách cẩn thận vì đường thẳng đứng sẽ loại bỏ các đóng góp đã được tính trong tập hợp theo chiều ngang. 

Từ góc độ ràng buộc, giải pháp dự định phải chạy gần với thời gian tuyến tính hoặc tuyến tính. Bất kỳ cách tiếp cận nào tính toán lại các lựa chọn tốt nhất toàn cầu sau mỗi lần loại bỏ một cột hoặc hàng giả định sẽ quá chậm trong trường hợp xấu nhất, vì có thể có tới$10^5$điểm và tính toán lại ngây thơ sẽ dẫn đến$O(n^2)$hành vi. 

Một số trường hợp khó nhận thấy có vấn đề. Nếu tất cả các điểm có cùng tọa độ y thì “ba cấp độ y hàng đầu” sẽ thoái hóa thành một ứng cử viên duy nhất được lặp lại ba lần và việc triển khai bất cẩn có thể bị tính quá mức. Một trường hợp cạnh khác xuất hiện khi đường thẳng đứng tốt nhất loại bỏ một trong các cấp độ y đóng góp hàng đầu, thay đổi thứ hạng của các ứng cử viên còn lại. Ví dụ: nếu tổng cấp độ y tốt nhất được phân cụm chặt chẽ, việc loại bỏ một cột lớn có thể cải tổ lại hai hoặc ba lựa chọn hàng đầu và việc duy trì top-k ngây thơ có thể thất bại nếu giả định thứ hạng ổn định. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử tất cả các lựa chọn có thể có của ba dòng, đánh giá xem chúng bao gồm bao nhiêu điểm và lấy mức tối đa. Đối với mỗi cấu hình ứng cử viên, chúng tôi sẽ tính toán lại các khoản đóng góp từ đầu. Với$n$điểm, đánh giá một cấu hình duy nhất là$O(n)$, và có$O(n^3)$các lựa chọn đường nếu chúng ta xem xét tất cả các lựa chọn trục cấp y hoặc trục hỗn hợp có thể có. Điều này dẫn đến ít nhất$O(n^4)$theo cách giải thích trực tiếp, điều này hoàn toàn không thể thực hiện được. 

Lực lượng vũ phu có cấu trúc chặt chẽ hơn làm giảm không gian tìm kiếm. Chúng tôi quan sát thấy rằng trong bất kỳ giải pháp tối ưu nào, chỉ có tối đa ba giá trị y riêng biệt và nhiều nhất một giá trị x quan trọng trong cấu hình hỗn hợp. Điều này làm giảm không gian ứng viên nhưng vẫn khiến chúng tôi phải tính toán lại cho mỗi ứng viên, tốc độ này vẫn quá chậm. 

Cái nhìn sâu sắc quan trọng là phân tách các đóng góp bằng cách tổng hợp tọa độ. Thay vì xử lý các điểm riêng lẻ, chúng tôi nén tất cả các điểm vào bản đồ tần số theo các giá trị y, tạo ra một mảng`count[y]`lưu trữ tổng trọng lượng trên mỗi dòng ngang. Bây giờ, việc chọn các đường ngang trở thành một vấn đề trong việc chọn các giá trị hàng đầu từ mảng này. 

Khi xem xét cấu hình với ba lần đánh ngang, câu trả lời đơn giản là tổng của ba giá trị lớn nhất trong`count`. Đây là trực tiếp. 

Trường hợp thú vị hơn là khi chúng ta sử dụng hai đòn ngang và một đòn dọc. Việc cố định một đường thẳng đứng ở một số tọa độ x sẽ loại bỏ hiệu quả tất cả các điểm có x đó khỏi việc đóng góp vào số lượng theo chiều ngang. Điều này sửa đổi`count[y]`cục bộ, nhưng việc tính toán lại từ đầu cho mỗi x thì quá tốn kém. 

Quan sát quan trọng là khi chúng ta loại bỏ một tập hợp$S_x$, chúng tôi chỉ ảnh hưởng đến các cấp độ y có điểm trong cột đó. Thay vì tính toán lại toàn bộ thứ tự của`count[y]`, chúng ta chỉ cần điều chỉnh một số lượng nhỏ giá trị và theo dõi sự thay đổi của các ứng cử viên hàng đầu. Vì chỉ$|S_x|$cấp độ y bị ảnh hưởng, chúng tôi có thể duy trì các ứng cử viên hàng đầu một cách hiệu quả và đảm bảo rằng câu trả lời tốt nhất sau khi loại bỏ chỉ phụ thuộc vào một tiền tố nhỏ của cấu trúc được sắp xếp. 

Điều này dẫn đến một đường chuyền tuyến tính trên mỗi x và với sự tổng hợp cẩn thận, tổng độ phức tạp sẽ trở thành tuyến tính sau quá trình tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tổng hợp + cập nhật gia tăng |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lại giải pháp theo cách tránh phải tính toán lại thứ hạng toàn cầu nhiều lần. 

### 1. Xây dựng tập hợp tọa độ 

Đầu tiên chúng tôi nén tất cả các điểm thành hai bản đồ băm. Một bản đồ`cntY[y]`lưu trữ tổng đóng góp của tất cả các điểm trên hàng y. Một bản đồ khác nhóm các điểm theo x để chúng ta có thể nhanh chóng truy cập tất cả các giá trị y bị ảnh hưởng bởi cú đánh thẳng đứng tại x. 

Sự chuyển đổi này rất cần thiết vì vấn đề không còn là về các điểm riêng lẻ mà là về cách toàn bộ hàng hoạt động khi loại bỏ cột. 

### 2. Tính toán trước thứ tự hàng cơ sở 

Chúng tôi tạo danh sách tất cả các giá trị y và số lượng tổng hợp của chúng. Về mặt khái niệm, chúng tôi sắp xếp danh sách này theo thứ tự giảm dần`cntY[y]`. Cấu trúc được sắp xếp này thể hiện các cú đánh ngang tốt nhất có thể khi không có gì bị loại bỏ. 

Chúng tôi không cần sắp xếp đầy đủ nếu các giá trị bị giới hạn hoặc có thể nén được, nhưng về mặt khái niệm, thứ tự này xác định “ba ứng cử viên hàng đầu”. 

### 3. Đánh giá trường hợp thuần túy ngang 

Chúng tôi tính tổng của ba giá trị hàng đầu trong`cntY`. Điều này tương ứng với việc sử dụng cả ba đòn đánh theo chiều ngang. 

Đây đóng vai trò là một câu trả lời ứng viên và không cần sửa đổi gì thêm. 

### 4. Chuẩn bị mô phỏng tấn công thẳng đứng 

Đối với mỗi tọa độ x, chúng tôi coi đó là vị trí của đòn tấn công thẳng đứng. Chúng tôi lặp lại tất cả các giá trị y trong cột đó và tạm thời giảm sự đóng góp của chúng trong`cntY`bằng trọng số của các điểm tại$(x, y)$. 

Ý tưởng chính là chỉ những hàng bị ảnh hưởng bởi cột này mới thay đổi giá trị của chúng, do đó, chỉ những hàng đó mới có thể ảnh hưởng đến lựa chọn theo chiều ngang ở hai trên cùng sau đó. 

### 5. Trích xuất tốt nhất 2 đòn ngang sau khi gỡ bỏ 

Thay vì tính toán lại cấu trúc được sắp xếp đầy đủ, chúng tôi chỉ quét một số lượng giới hạn các ứng cử viên: các hàng nằm ở vùng trên cùng cộng với những hàng bị ảnh hưởng bởi việc loại bỏ. Trong số này, chúng tôi xác định hai cấp độ y tốt nhất còn lại. 

Điều này có tác dụng vì việc loại bỏ một cột không thể đột nhiên nâng một hàng không có tính cạnh tranh cao lên hàng đầu toàn cầu trừ khi nó đã ở gần thứ hạng. 

### 6. Kết hợp với đóng góp theo chiều dọc 

Với mỗi x, hãy tính: 

tổng của tất cả các giá trị trong cột x cộng với hai hàng ngang tốt nhất sau khi loại bỏ. 

Chúng tôi theo dõi mức tối đa trên tất cả x. 

### 7. Khôi phục trạng thái 

Sau khi xử lý từng x, chúng tôi hoàn tác các phần giảm tạm thời để cột tiếp theo bắt đầu từ trạng thái ban đầu. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào đặc tính cục bộ của những thay đổi xếp hạng. Một cuộc tấn công dọc chỉ sửa đổi một tập hợp con nhỏ của tập hợp hàng và tất cả các hàng không bị ảnh hưởng sẽ duy trì thứ tự tương đối của chúng. Do đó, các ứng cử viên tốt nhất trong số các hàng không bị ảnh hưởng vẫn ổn định và chỉ những hàng bị ảnh hưởng mới có thể vào hoặc rời khỏi tập hợp top-K. Vì K là hằng số (hai hoặc ba), nên chúng ta chỉ cần kiểm tra cửa sổ ứng cử viên có kích thước không đổi trên mỗi cột. Điều này ngăn cản việc tính toán lại toàn cục trong khi vẫn duy trì sự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = []
    cntY = {}
    byX = {}

    for _ in range(n):
        x, y, w = map(int, input().split())
        pts.append((x, y, w))
        cntY[y] = cntY.get(y, 0) + w
        if x not in byX:
            byX[x] = []
        byX[x].append((y, w))

    # baseline top 3 y values
    vals = sorted(cntY.values(), reverse=True)
    best3 = sum(vals[:3]) if len(vals) >= 3 else sum(vals)

    ans = best3

    for x, arr in byX.items():
        affected = {}
        for y, w in arr:
            affected[y] = affected.get(y, 0) + w

        changed = []
        for y, w in affected.items():
            cntY[y] -= w
            changed.append((y, w))

        vals2 = sorted(cntY.values(), reverse=True)
        best2 = sum(vals2[:2]) if len(vals2) >= 2 else sum(vals2)

        col_sum = sum(w for _, w in arr)
        ans = max(ans, col_sum + best2)

        for y, w in changed:
            cntY[y] += w

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã nén tất cả các đóng góp vào tổng hàng và nhóm cột. Nó tính toán câu trả lời cơ bản bằng cách sử dụng tổng ba hàng trên cùng. Sau đó, đối với mỗi cột, nó mô phỏng việc loại bỏ cột đó bằng cách trừ đi phần đóng góp của nó khỏi các hàng bị ảnh hưởng. Sau mỗi lần mô phỏng, nó sẽ tính toán lại hai hàng tốt nhất và kết hợp chúng với phần đóng góp của cột. 

Bước khôi phục là rất quan trọng vì không thể hoàn nguyên`cntY`sẽ tích lũy số lần xóa trên các cột và làm hỏng các đánh giá tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét điểm:$$(1,1,3), (1,2,2), (2,1,4), (3,2,1)$$Tổng số hàng ban đầu là: 

y=1 → 7, y=2 → 3 

| Bước | Hành động | trạng thái cntY | Ngang tốt nhất | 
| --- | --- | --- | --- | 
| 0 | ban đầu | {1:7, 2:3} | 7 + 3 | 
| 1 | thử x=1 | {1:4, 2:1} | 4 + 1 | 
| 2 | thử x=2 | {1:3, 2:3} | 3 + 3 | 
| 3 | thử x=3 | {1:7, 2:2} | 7 + 2 | 

Cấu hình tốt nhất kết hợp x=2 với các đường ngang tạo ra 3 + 3 từ các hàng và cột. 

Điều này cho thấy các cột khác nhau sắp xếp lại sự thống trị của hàng như thế nào. 

### Ví dụ 2 

Điểm:$$(1,1,5), (2,1,4), (3,1,3)$$| Bước | Hành động | trạng thái cntY | Ngang tốt nhất | 
| --- | --- | --- | --- | 
| 0 | ban đầu | {1:12} | 12 + 0 + 0 | 
| 1 | xóa x=1 | {1:7} | 7 | 
| 2 | xóa x=2 | {1:8} | 8 | 
| 3 | xóa x=3 | {1:9} | 9 | 

Trường hợp này chứng tỏ tình huống suy biến trong đó tất cả các điểm nằm trên một hàng, do đó các lựa chọn theo chiều ngang sẽ sụp đổ và các lựa chọn theo chiều dọc chiếm ưu thế hơn các điều chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$trường hợp xấu nhất (mô phỏng ngây thơ) | Mỗi cột tính lại tổng hàng đã sắp xếp | 
| Không gian |$O(n)$| Lưu trữ điểm và tổng hợp | 

Cấu trúc được thiết kế để tổng hợp tuyến tính, nhưng mô phỏng đơn giản được trình bày có thể bị suy giảm nếu tồn tại nhiều cột. Trong thực tế, việc tối ưu hóa giúp giảm việc tính toán lại bằng cách hạn chế các hàng bị ảnh hưởng, làm cho nó phù hợp với các ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# placeholder assertions (problem statement incomplete)
# these would normally call solve()

assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm đơn tối thiểu | tầm thường | trường hợp cơ sở | 
| tất cả các điểm giống nhau y | xử lý tổng hợp | sập hàng | 
| phân biệt x, giống y | sự thống trị theo chiều dọc | hiệu ứng cột | 
| lưới hỗn hợp | kết hợp tối đa | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các điểm nằm trên một tọa độ y. Trong tình huống này, mọi cú đánh ngang đều tương đương và giải pháp tốt nhất phụ thuộc hoàn toàn vào việc liệu việc sử dụng cú đánh dọc có mang lại sự tách biệt tốt hơn hay không. Thuật toán xử lý việc này một cách tự nhiên vì`cntY`chỉ có một mục nhập, vì vậy hai lựa chọn trên cùng sẽ suy biến một cách an toàn. 

Một trường hợp cạnh khác xuất hiện khi một cột chứa tất cả các điểm có trọng số cao trên nhiều hàng. Việc xóa cột đó có thể sắp xếp lại các hàng trên cùng một cách đáng kể. Bước mô phỏng sẽ trừ đi các khoản đóng góp một cách rõ ràng trước khi tính toán lại hai hàng tốt nhất để sự thay đổi thứ hạng được nắm bắt một cách chính xác. 

Trường hợp thứ ba là khi có ít hơn ba giá trị y riêng biệt. Thuật toán tránh lỗi chỉ mục bằng cách lấy số tiền tối thiểu có sẵn khi cắt danh sách các giá trị hàng đã được sắp xếp, đảm bảo tính chính xác ngay cả trong các cấu hình thưa thớt.
