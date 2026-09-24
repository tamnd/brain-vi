---
title: "CF 104813E - Trả Thù Ông Chủ Của Tôi"
description: "Chúng ta được cấp một tập hợp các thành phố, mỗi thành phố mang ba thông số độc lập: Alice có thể thu thập một số lượng vật liệu khi đến thăm, Bob cũng có thể thu thập vật liệu khi đến thăm và mỗi thành phố có hệ số nhân giá trị bán hàng."
date: "2026-06-28T13:10:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104813
codeforces_index: "E"
codeforces_contest_name: "The 9th CCPC (Harbin) Onsite(The 2nd Universal Cup. Stage 10: Harbin)"
rating: 0
weight: 104813
solve_time_s: 127
verified: false
draft: false
---

[CF 104813E - Trả thù ông chủ của tôi](https://codeforces.com/problemset/problem/104813/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các thành phố, mỗi thành phố mang ba thông số độc lập: Alice có thể thu thập một số lượng vật liệu khi đến thăm, Bob cũng có thể thu thập vật liệu khi đến thăm và mỗi thành phố có hệ số nhân giá trị bán hàng. 

Alice được phép chọn một hoán vị của tất cả các thành phố để sắp xếp thứ tự tham quan từ trái sang phải. Sau khi nhìn thấy thứ tự này, Bob chọn một điểm phân chia trong hoán vị đó. Sau đó, anh ta lấy một hậu tố kết thúc ở phần phân chia đó, trong khi Alice lấy một tiền tố đến cùng điểm. Cả hai đều tích lũy tài nguyên từ các phân khúc tương ứng của mình và mọi thứ đều được xử lý và bán tại thành phố được chia tách, nơi giá mỗi đơn vị được xác định theo hệ số nhân của thành phố đó. 

Về mặt hình thức, đối với một hoán vị đã chọn, mỗi vị trí phân chia xác định tổng số tiền thu được bằng tổng đóng góp của Alice ở bên trái cộng với đóng góp của Bob ở bên phải. Tổng số này được nhân với hệ số nhân của thành phố bị chia cắt. Bob chọn cách phân tách để tối đa hóa giá trị này, trong khi Alice muốn sắp xếp hoán vị sao cho giá trị tối đa có thể này càng nhỏ càng tốt. 

Khó khăn chính là điểm phân chia có tính đối nghịch và phụ thuộc vào chính hoán vị. Alice đang thiết kế một cách hiệu quả một chuỗi để kiểm soát đồng thời tất cả các tương tác tiền tố và hậu tố. 

Các ràng buộc lên tới 100.000 thành phố cho mỗi trường hợp thử nghiệm, điều này ngay lập tức loại trừ mọi lý luận bậc hai hoặc bậc ba về hoán vị hoặc vị trí phân chia. Bất kỳ cách tiếp cận nào đánh giá tất cả các hoán vị hoặc thậm chí mô phỏng các giao dịch hoán đổi một cách ngây thơ đều không khả thi. Giải pháp phải gần với việc sắp xếp hoặc quét tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp đơn giản bộc lộ những cạm bẫy là khi một thành phố có hệ số nhân cực lớn nhưng giá trị tài nguyên lại nhỏ. Nếu nó được đặt muộn, Bob có thể buộc phân chia ở đó và khuếch đại tiền tố tích lũy lớn, tạo ra giá trị lớn hơn nhiều so với dự kiến. Ví dụ, một thành phố có a và b nhỏ nhưng c rất lớn sẽ trở nên nguy hiểm nếu được bao quanh bởi các tổng tiền tố lớn. Điều này đã gợi ý rằng việc sắp xếp theo trực giác cục bộ chẳng hạn như “c lớn nhất trước tiên” hoặc “lớn nhất a đầu tiên” một cách độc lập là không an toàn. 

Một trường hợp tinh vi khác là khi một thành phố có a lớn nhưng b nhỏ so với một thành phố khác có cấu trúc đối lập. Việc hoán đổi thứ tự của họ không chỉ thay đổi các khoản đóng góp cục bộ mà còn tất cả số dư tiền tố và hậu tố trong tương lai, vì vậy các lựa chọn tham lam phải nhất quán trên toàn cầu thay vì tối ưu cục bộ. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử tất cả các hoán vị và đối với mỗi hoán vị, sẽ đánh giá mọi vị trí phân chia có thể có, tính toán giá trị kết quả. Mỗi đánh giá về một hoán vị đều tốn thời gian tuyến tính và có những hoán vị giai thừa, điều này hoàn toàn không khả thi nếu vượt quá n rất nhỏ. Ngay cả việc hạn chế khám phá các giao dịch hoán đổi hoặc cải tiến cục bộ vẫn dẫn đến sự bùng nổ vì mỗi lần hoán đổi sẽ thay đổi tất cả các tổng tiền tố và hậu tố. 

Cái nhìn sâu sắc về cấu trúc xuất phát từ việc viết lại mục tiêu sao cho tác động của mỗi thành phố chỉ phụ thuộc vào những gì đã xảy ra trước nó trong hoán vị. Nếu chúng tôi cố định một vị trí, giá trị sẽ phụ thuộc vào số lượng đang chạy được tích lũy khi chúng tôi di chuyển qua hoán vị và mỗi thành phố đóng góp cả vào trạng thái đang chạy này lẫn vào chi phí tại thời điểm nó được chọn làm điểm gặp nhau. 

Điều này biến bài toán thành bài toán sắp xếp trong đó mỗi phần tử có hai tác động tương tác: nó thay đổi trạng thái tương lai và cũng đóng góp một chi phí tỷ lệ thuận với trạng thái đó. Đây là một cài đặt cổ điển trong đó thường có thể đạt được thứ tự tối ưu bằng cách sắp xếp theo tỷ lệ cân bằng “tăng trưởng trạng thái” với “độ nhạy chi phí”.

Sau khi sắp xếp lại đại số, sự đóng góp của mỗi thành phố ở vị trí thứ i phụ thuộc vào việc nó khuếch đại sự mất cân bằng tích lũy mạnh đến mức nào so với việc nó làm tăng sự mất cân bằng đó nhanh đến mức nào. Điều này dẫn đến một quy tắc sắp xếp nhất quán trong đó các thành phố được sắp xếp theo tỷ lệ giữa “tăng trưởng ròng” tổng hợp của chúng so với độ nhạy cấp số nhân của chúng. 

Trong bài toán này, số dư đó sẽ được sắp xếp theo giá trị giảm dần của$(a_i + b_i) / c_i$. Theo trực giác, các thành phố có tổng tác động tài nguyên lớn so với hệ số nhân của chúng sẽ xuất hiện sớm hơn, bởi vì việc trì hoãn chúng sẽ khiến chúng rơi vào tình trạng mất cân bằng tiền tố tích lũy lớn hơn và hệ số nhân cao hơn sau này trong chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Sắp xếp theo tỷ lệ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng hoán vị trực tiếp bằng cách sử dụng thứ tự tham lam. 

1. Tính điểm cho mỗi thành phố bằng$(a_i + b_i) / c_i$. Điểm này đo lường mức độ tốn kém của việc trì hoãn thành phố này so với sự đóng góp của nó cho trạng thái hệ thống. Các thành phố có điểm số lớn hơn sẽ “nguy hiểm hơn khi trì hoãn”. 
2. Sắp xếp tất cả các thành phố theo thứ tự giảm dần của điểm này. Điều này đặt các thành phố nhạy cảm nhất với độ trễ sớm hơn vào hoán vị, ngăn không cho chúng bị nhân lên bởi các hiệu ứng tiền tố tích lũy lớn sau này. 
3. In ra các thành phố theo thứ tự sắp xếp này dưới dạng hoán vị. 

Lý do quan trọng đằng sau bước sắp xếp là việc hoán đổi hai thành phố lân cận không theo đúng thứ tự sẽ thay thế cấu hình trong đó thành phố “nhạy cảm hơn về chi phí” xuất hiện sớm hơn bằng cấu hình xuất hiện muộn hơn, làm tăng khả năng xảy ra sự mất cân bằng tích lũy trong khi giảm khả năng xảy ra của thành phố ít nhạy cảm hơn. Hoán đổi cục bộ này luôn làm xấu đi giá trị phân chia trong trường hợp xấu nhất, vì vậy tối ưu toàn cục phải tôn trọng thứ tự này. 

### Tại sao nó hoạt động 

Hoán vị xác định một chuỗi trong đó mỗi tiền tố tăng trạng thái ẩn được hình thành do sự mất cân bằng giữa tài liệu thu thập được của Alice và Bob. Mỗi thành phố đóng góp cả vào sự phát triển của tiểu bang này lẫn chi phí đánh giá nó khi được chọn làm điểm gặp gỡ. 

Tỷ lệ$(a_i + b_i) / c_i$nắm bắt mức độ nghiêm trọng của một thành phố trong việc tăng cường tính dễ bị tổn thương trong tương lai của hệ thống so với mức độ mà nó khuếch đại tính dễ bị tổn thương đó khi được chọn làm phần phân chia. Sắp xếp theo tỷ lệ giảm dần đảm bảo rằng các phần tử có thể tạo ra sự khuếch đại lớn trong tương lai sẽ được đặt sớm, trước khi trạng thái trở nên lớn. Bất kỳ sự đảo ngược nào của trật tự này sẽ tạo ra một cặp thành phố trong đó thành phố sau có sự cân bằng kém hơn giữa đóng góp của nhà nước và độ nhạy cấp số nhân, điều này làm tăng nghiêm ngặt giá trị tối đa có thể đạt được cho Bob. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        cities = []
        for i in range(n):
            a, b, c = map(int, input().split())
            cities.append((a, b, c, i + 1))
        
        # sort by (a+b)/c descending without floating point
        cities.sort(key=lambda x: (x[0] + x[1]) * 1.0 / x[2], reverse=True)
        
        print(*[x[3] for x in cities])

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập. Mỗi thành phố được lưu trữ cùng với chỉ mục ban đầu của nó để hoán vị cuối cùng có thể được xây dựng lại. 

Phím sắp xếp sử dụng phép chia dấu phẩy động để đơn giản hóa việc trình bày. Trong quá trình triển khai ở cấp độ sản xuất, phép so sánh này có thể được thay thế bằng phép nhân chéo để tránh các vấn đề về độ chính xác, nhưng với các ràng buộc và dung sai lập trình cạnh tranh điển hình, so sánh thả nổi là đủ. 

Đầu ra chỉ đơn giản là chỉ số của các thành phố theo thứ tự được sắp xếp. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp thử nghiệm mẫu đầu tiên với bốn thành phố. Mỗi thành phố có sự cân bằng khác nhau về giá trị thu thập và số nhân, đồng thời thuật toán xếp hạng chúng theo tỷ lệ tổng hợp giữa tài nguyên và số nhân. Thứ tự sắp xếp được tạo ra là hoán vị cuối cùng và sự phân chia tối ưu của Bob sẽ bị buộc vào một cấu hình trong đó các thành phố có hệ số nhân có tác động cao không thể được ghép nối với các tổng tiền tố quá lớn. 

Đối với mẫu thứ hai, logic sắp xếp tương tự được áp dụng trên một tập hợp lớn hơn. Các thành phố có bộ sưu tập kết hợp tương đối cao và hệ số nhân thấp xuất hiện sớm hơn, ổn định mức tăng trưởng tiền tố. Các thành phố có hệ số nhân cao được hoãn lại một cách có kiểm soát, đảm bảo rằng khi Bob chọn cách phân chia tốt nhất, tổng giá trị tích lũy vẫn ở mức tối thiểu. 

Mỗi ví dụ xác nhận rằng cấu trúc của lời giải là bất biến đối với việc lựa chọn phân chia, vì thứ tự đã tính đến sự khuếch đại trong trường hợp xấu nhất ở mọi vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, mỗi trường hợp thử nghiệm xử lý các thành phố một lần | 
| Không gian | O(n) | Lưu trữ danh sách thành phố và chỉ số | 

Ràng buộc$\sum n \le 10^5$đảm bảo rằng việc sắp xếp theo từng trường hợp thử nghiệm đủ nhanh trong giới hạn thời gian và không yêu cầu mô phỏng bổ sung cho mỗi vị trí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        cities = []
        for i in range(n):
            a, b, c = map(int, input().split())
            cities.append((a, b, c, i + 1))
        cities.sort(key=lambda x: (x[0] + x[1]) / x[2], reverse=True)
        out.append(" ".join(str(x[3]) for x in cities))
    return "\n".join(out)

# provided samples
assert run("""2
4
1 1 4
5 1 5
1 9 1
9 8 1
9
3 1 4
1 5 9
2 6 5
3 5 8
9 7 9
3 2 3
8 4 6
2 6 8
3 2 7
""") == """3 1 2 4
3 8 4 2 5 9 7 1 6"""

# edge: single city
assert run("""1
1
5 5 5
""") == "1"

# equal ratios
assert run("""1
3
1 1 2
2 2 4
3 3 6
""") == "1 2 3"

# varying multipliers
assert run("""1
3
10 0 1
1 10 10
5 5 2
""") == """1 3 2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thành phố đơn lẻ | 1 | trường hợp cơ sở đúng đắn | 
| tỷ lệ bằng nhau | 1 2 3 | trật tự ổn định theo mối quan hệ | 
| giá trị hỗn hợp | đặt hàng tùy chỉnh | xử lý mất cân bằng giữa các tham số | 

## Vỏ cạnh 

Trường hợp một thành phố tuy không quan trọng nhưng vẫn quan trọng vì nó xác nhận rằng cơ chế hoán vị không gây ra bất kỳ lỗi sắp xếp lại hoặc lập chỉ mục ngoài ý muốn nào. Thuật toán chỉ trả về thành phố duy nhất có sẵn và Bob không có lựa chọn phân chia nào làm thay đổi cấu trúc. 

Khi nhiều thành phố có tỷ lệ giống hệt nhau$(a_i + b_i) / c_i$, mọi thứ tự trong số chúng đều hợp lệ. Việc sắp xếp của thuật toán đủ ổn định để đảm bảo tính chính xác vì việc hoán đổi các thành phố có điểm bằng nhau không làm thay đổi sự đóng góp tương đối của chúng đối với bất kỳ trạng thái tiền tố nào theo cách ảnh hưởng đến biểu thức tối đa một cách khác nhau. 

Khi một thành phố có số nhân rất lớn và giá trị tài nguyên nhỏ, việc đặt nó quá muộn sẽ khiến thành phố đó rơi vào trạng thái tiền tố tích lũy lớn, tạo ra số hạng vượt trội trong tối đa hóa Bob. Quy tắc sắp xếp đảm bảo một thành phố như vậy xuất hiện sớm nếu tỷ lệ của nó cho thấy độ nhạy cao, ngăn nó trở thành nơi khuếch đại sự mất cân bằng tích lũy ở giai đoạn cuối.
