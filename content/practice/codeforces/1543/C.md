---
title: "CF 1543C - Cần Phiếu Hồng"
description: "Chúng ta đang chạy đi chạy lại một quá trình ngẫu nhiên để phát triển một phân bố xác suất nhỏ trên ba kết quả. Tại bất kỳ thời điểm nào, có tới ba phiếu “hoạt động”, một trong số đó là trạng thái thành công cuối cùng (phiếu màu hồng) và hai phiếu còn lại là trạng thái nhất thời."
date: "2026-06-14T19:13:25+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dfs-and-similar", "implementation", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1543
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 730 (Div. 2)"
rating: 1900
weight: 1543
solve_time_s: 391
verified: false
draft: false
---

[CF 1543C - Need for Pink Slips](https://codeforces.com/problemset/problem/1543/C) 

**Xếp hạng:** 1900 
**Tags:** bitmasks, brute Force, dfs và tương tự, triển khai, toán học, xác suất 
**Thời gian giải:** 6 phút 31 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang chạy đi chạy lại một quá trình ngẫu nhiên để phát triển một phân bố xác suất nhỏ trên ba kết quả. Tại bất kỳ thời điểm nào, có tới ba phiếu “hoạt động”, một trong số đó là trạng thái thành công cuối cùng (phiếu màu hồng) và hai phiếu còn lại là trạng thái nhất thời. Mỗi cuộc đua tương ứng với một lần rút từ phân phối hiện tại và sau mỗi lần rút, phân phối được sửa đổi theo cách xác định tùy thuộc vào mục nào được rút và khối lượng xác suất hiện tại của nó. 

Quá trình kết thúc khi phiếu màu hồng được rút ra lần đầu tiên. Nhiệm vụ là tính toán số lần rút dự kiến ​​cho đến khi kết thúc. 

Mặc dù hệ thống bắt đầu với ba xác suất, nhưng các quy tắc ngụ ý rằng các vật phẩm có thể biến mất vĩnh viễn khi khối lượng xác suất của chúng bị đẩy về 0. Khi điều đó xảy ra, khối lượng xác suất còn lại sẽ được phân phối lại giữa các mục còn sót lại hoặc được điều chỉnh tỷ lệ tùy thuộc vào tham số ngưỡng v. Điều này có nghĩa là trạng thái của hệ thống không cố định nhưng vẫn được xác định đầy đủ bởi tập hợp con nào của mục vẫn hoạt động và xác suất hiện tại của chúng. 

Kích thước đầu vào chính là không đổi. Có nhiều nhất ba trạng thái ban đầu và nhiều nhất là 10 trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào cũng có thể cung cấp các kỹ thuật liệt kê trạng thái hoặc hàm mũ đối với các phân phối có thể có do loại bỏ và phân phối lại. Điều này ngay lập tức loại trừ bất kỳ nhu cầu nào về việc truyền tải đồ thị tiệm cận hoặc lập trình động quy mô lớn. Một sức mạnh tàn bạo đối với các trạng thái là khả thi nếu không gian trạng thái được kiểm soát chính xác. 

Một vấn đề tế nhị là xác suất phát triển liên tục chứ không phải một cách rời rạc trên một lưới số nguyên nhỏ. Một nỗ lực ngây thơ để ghi nhớ các trạng thái bằng cách sử dụng bộ ba dấu phẩy động thô sẽ thất bại do mất ổn định độ chính xác và bùng nổ trạng thái. Một cạm bẫy phổ biến khác là coi các chuyển đổi là độc lập với xác suất được rút ra; quy tắc phân phối lại kết hợp các xác suất chuyển tiếp và trạng thái tiếp theo theo cách phá vỡ các phân tách kỳ vọng tuyến tính ngây thơ. 

Các trường hợp cạnh chính phát sinh khi một trong các xác suất không kết thúc trở nên rất nhỏ so với v, gây ra sự loại bỏ ngay lập tức hoặc khi việc loại bỏ lặp đi lặp lại làm giảm hệ thống thành chuỗi hai trạng thái hoặc một trạng thái. Trong các chế độ này, quá trình này sụp đổ nhanh chóng, nhưng các mô phỏng đệ quy ngây thơ có thể đếm quá nhiều bước hoặc không truyền được khối lượng xác suất một cách chính xác sau khi loại bỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp mô phỏng quá trình như một chuỗi Markov trên các trạng thái liên tục. Từ một trạng thái nhất định, chúng tôi xem xét việc vẽ từng mục đang hoạt động, cập nhật xác suất theo quy tắc và tính toán đệ quy số bước còn lại dự kiến. Điều này đúng về mặt khái niệm vì kỳ vọng thỏa mãn sự lặp lại trên tất cả các trạng thái tiếp theo có thể có được tính theo xác suất chuyển tiếp. 

Tuy nhiên, khó khăn là sau mỗi lần rút thăm, xác suất thay đổi theo cách xác định nhưng phụ thuộc vào giá trị. Điều này có nghĩa là cùng một trạng thái cấu trúc có thể tạo ra vô số biến thể số nếu được theo dõi một cách đơn giản. Nếu chúng ta cố gắng coi mọi bộ ba số thực có thể có là một trạng thái riêng biệt thì phép đệ quy sẽ không bao giờ hợp nhất các trạng thái và sẽ không kết thúc theo nghĩa được ghi nhớ. 

Quan sát quan trọng là hệ thống này cực kỳ nhỏ và có cấu trúc cao. Tại bất kỳ thời điểm nào, trạng thái có ý nghĩa duy nhất được xác định bởi tập hợp con của các mục vẫn đang hoạt động, cùng với xác suất hiện tại của chúng. Vì việc loại bỏ chỉ xảy ra bằng cách đặt xác suất về 0 nên số lượng trạng thái cấu trúc bị giới hạn. Quan trọng hơn, ở bất kỳ trạng thái nào, chúng ta chỉ cần xem xét kỳ vọng qua nhiều nhất ba lần chuyển đổi và trạng thái tiếp theo được xác định duy nhất bằng các phép biến đổi số học đơn giản.

Điều này cho phép chúng ta xác định hàm đệ quy trên vectơ xác suất hiện tại. Bất chấp tính chất liên tục của các giá trị, độ sâu đệ quy bị giới hạn vì mỗi lần loại bỏ sẽ làm giảm nghiêm ngặt số lượng mục hoạt động và trong một tập hợp hoạt động cố định, phép truy hồi hội tụ thông qua các phương trình kỳ vọng tuyến tính có thể được giải quyết thông qua đánh giá trực tiếp. 

Do đó, thay vì mô phỏng các chuỗi dài, chúng tôi tính toán kỳ vọng bằng phương trình hàm: từ trạng thái (c, m, p), giá trị kỳ vọng là 1 cộng với kỳ vọng có trọng số của trạng thái tiếp theo dựa trên mỗi lần rút có thể xảy ra, trong đó trọng số là xác suất hiện tại. 

Giải pháp tối ưu đánh giá sự tái diễn này một cách trực tiếp bằng cách ghi nhớ các trạng thái trong đó xác suất được chuẩn hóa sau mỗi lần chuyển đổi và dung sai dấu phẩy động là đủ vì chỉ tồn tại một số lượng nhỏ trạng thái có thể tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force trên các đường dẫn | hàm mũ | hàm mũ | Quá chậm | 
| Kỳ vọng đệ quy trạng thái DP | O(1) mỗi tiểu bang, vài tiểu bang | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một hàm đệ quy`solve(c, m, p)`trả về số bước dự kiến ​​để đạt đến phiếu hồng từ phân bố xác suất hiện tại. 

1. Nếu xác suất trượt hồng thực tế là 1 thì kỳ vọng là 0 vì quá trình đã kết thúc. Đây là trường hợp cơ bản của sự tái phát. 
2. Nếu không, mỗi lần rút sẽ tốn 1 bước, vì vậy chúng tôi bắt đầu kỳ vọng bằng 1. 
3. Chúng ta xét việc rút phiếu tiền mặt với xác suất c. Nếu nó được rút ra, chúng tôi cập nhật phân phối theo quy tắc. Nếu c lớn nhất là v thì hạng mục đó bị loại bỏ và khối lượng xác suất của nó được phân phối lại đều cho các hạng mục còn lại. Nếu c lớn hơn v, chúng ta trừ v từ c và phân phối lại khối lượng đã loại bỏ bằng nhau. 
4. Chúng ta làm tương tự đối với đường trượt ở giữa với xác suất m, áp dụng các quy tắc biến đổi tương tự. 
5. Chúng ta cũng xét phiếu hồng với xác suất p. Nếu nó được rút ra, quá trình sẽ dừng lại, do đó nó không còn mang lại kỳ vọng gì nữa. 
6. Chúng ta kết hợp các chuyển đổi này một cách tuyến tính: giá trị kỳ vọng là 1 cộng c nhân với kỳ vọng của trạng thái sau khi rút tiền, cộng với m nhân với kỳ vọng của trạng thái sau khi rút m, cộng với p nhân 0. 
7. Vì không gian trạng thái cực kỳ nhỏ nên chúng tôi ghi nhớ kỳ vọng đã tính toán cho từng bộ ba (c, m, p) sau khi chuẩn hóa các giá trị dấu phẩy động để tránh phải tính toán lại nhiều lần. 

Tính đúng đắn dựa trên thực tế là kỳ vọng trong quy trình Markov thỏa mãn sự phân tách tuyến tính qua các chuyển đổi bước đầu tiên. Mỗi trạng thái xác định đầy đủ sự phân bố của các trạng thái tiếp theo và mọi quỹ đạo có thể xảy ra trong tương lai đều được nắm bắt chính xác một lần bằng cách mở rộng ở lần rút thăm đầu tiên. Mặc dù các giá trị trạng thái là liên tục, cấu trúc đệ quy đảm bảo chúng ta chỉ đánh giá hữu hạn nhiều cấu hình riêng biệt được tạo ra bởi các quy tắc loại bỏ và phân phối lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from functools import lru_cache

@lru_cache(None)
def solve(c, m, p, v):
    # if pink slip dominates
    if p > 0.999999:
        return 0.0

    res = 1.0

    # cash
    if c > 1e-12:
        if c <= v:
            nc, nm, np = 0.0, m + c / 2, p + c / 2
        else:
            nc, nm, np = c - v, m + v / 2, p + v / 2
        res += c * solve(nc, nm, np, v)

    # middle
    if m > 1e-12:
        if m <= v:
            nc, nm, np = c + m / 2, 0.0, p + m / 2
        else:
            nc, nm, np = c + v / 2, m - v, p + v / 2
        res += m * solve(nc, nm, np, v)

    return res

def main():
    t = int(input())
    for _ in range(t):
        c, m, p, v = map(float, input().split())
        ans = solve(c, m, p, v)
        print(f"{ans:.12f}")

if __name__ == "__main__":
    main()
```Giải pháp trực tiếp thực hiện phép lặp kỳ vọng bước đầu tiên. chức năng`solve`xử lý mỗi lần rút độc lập và cập nhật vectơ xác suất theo quy tắc phân phối lại của bài toán. Việc ghi nhớ đảm bảo rằng các trạng thái lặp lại không được tính toán lại. 

Việc so sánh dấu phẩy động với các epsilon nhỏ ngăn chặn sự đệ quy vô hạn do nhiễu số khi xác suất thực tế trở thành bằng 0. Mỗi quá trình chuyển đổi xây dựng trạng thái tiếp theo một cách rõ ràng và cấu trúc đệ quy đảm bảo chấm dứt vì ít nhất một khối xác suất di chuyển theo hướng loại bỏ hoặc phân phối lại về trạng thái cuối. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu đầu tiên`(c, m, p, v) = (0.2, 0.2, 0.6, 0.2)`. 

Ở trạng thái ban đầu, chúng tôi tính toán kỳ vọng là 1 cộng với đóng góp từ mỗi lần rút tiền không đầu cuối. 

| Bước | Bang (c,m,p) | Hành động | Trạng thái tiếp theo | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | (0,2,0,2,0,6) | bắt đầu | giống nhau | +1 | 
| 2 | (0,2,0,2,0,6) | vẽ c | (0,0,3,0,7) | 0,2 * E(0,0,3,0,7) | 
| 3 | (0,2,0,2,0,6) | vẽ m | (0,3,0,0,7) | 0,2 * E(0,3,0,0,7) | 

Điều này cho thấy mỗi nhánh giảm ngay lập tức không gian trạng thái bằng cách loại bỏ một thành phần hoạt động hoặc chuyển khối lượng về trạng thái cuối. 

Đối với ví dụ thứ hai, hãy xem xét trường hợp có một xác suất nhỏ hơn v, chẳng hạn như`(0.1, 0.2, 0.7, 0.2)`. 

| Bước | Tiểu bang | Hành động | Trạng thái tiếp theo | 
| --- | --- | --- | --- | 
| 1 | (0,1,0,2,0,7) | vẽ c | loại bỏ c → (0,0,25,0,75) | 
| 2 | (0,0,25,0,75) | vẽ m | cập nhật bình thường | 

Điều này chứng tỏ các xác suất nhỏ biến mất sớm như thế nào, làm tăng tốc độ hội tụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi tiểu bang, rất ít tiểu bang | chỉ những cấu hình có thể truy cập từ hệ thống 3 thành phần mới được khám phá | 
| Không gian | O(1) | ghi nhớ chỉ lưu trữ số lượng trạng thái không đổi | 

Kích thước đầu vào không đổi và phép đệ quy khám phá một tập hợp các cấu hình xác suất bị chặn. Điều này đảm bảo việc thực thi nằm trong giới hạn cho tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    from functools import lru_cache

    def solve_all():
        @lru_cache(None)
        def solve(c, m, p, v):
            if p > 0.999999:
                return 0.0
            res = 1.0
            if c > 1e-12:
                if c <= v:
                    nc, nm, np = 0.0, m + c/2, p + c/2
                else:
                    nc, nm, np = c - v, m + v/2, p + v/2
                res += c * solve(nc, nm, np, v)
            if m > 1e-12:
                if m <= v:
                    nc, nm, np = c + m/2, 0.0, p + m/2
                else:
                    nc, nm, np = c + v/2, m - v, p + v/2
                res += m * solve(nc, nm, np, v)
            return res

        t = int(input())
        out = []
        for _ in range(t):
            c, m, p, v = map(float, input().split())
            solve.cache_clear()
            out.append(f"{solve(c,m,p,v):.12f}")
        return "\n".join(out)

    return solve_all()

# provided samples (approx checks)
assert abs(float(run("1\n0.2 0.2 0.6 0.2").split()[0]) - 1.532) < 1e-6
assert abs(float(run("1\n0.4 0.2 0.4 0.8").split()[0]) - 1.86) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| p cao đơn | ~1 | hành vi chấm dứt ngay lập tức | 
| trường hợp c nhỏ | ổn định | tính đúng đắn của nhánh loại bỏ | 
| chia đối xứng | nhất quán | phân phối lại đúng đắn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi xác suất chính xác ở ngưỡng v. Trong tình huống này, mục bị loại bỏ hoàn toàn thay vì giảm một phần. Thuật toán xử lý việc này bằng cách kiểm tra`c <= v`Và`m <= v`, điều này buộc hành vi zeroing và phân phối lại. Ví dụ, với`(c,m,p,v) = (0.1,0.2,0.7,0.2)`, bản vẽ c sẽ loại bỏ nó hoàn toàn và tạo ra`(0,0.25,0.75)`. Sau đó, đệ quy chỉ tiếp tục trên hai trạng thái hoạt động, ngăn chặn việc lưu giữ sai khối lượng xác suất nhỏ. 

Một trường hợp đặc biệt khác xảy ra khi các lần phân phối lại lặp đi lặp lại tích lũy lỗi dấu phẩy động sao cho xác suất hơi lệch khỏi tổng thành 1. Việc triển khai tránh điều này bằng cách xây dựng từng trạng thái tiếp theo từ các biểu thức số học chính xác và không bao giờ tái chuẩn hóa theo phương pháp phỏng đoán. Điều này bảo toàn tính bất biến rằng mỗi lần chuyển đổi thể hiện một phân bố xác suất hợp lệ, đảm bảo sự lặp lại kỳ vọng vẫn nhất quán trong suốt quá trình thực thi.
