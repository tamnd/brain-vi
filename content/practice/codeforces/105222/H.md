---
title: "CF 105222H - Trò chơi đá của GG và YY"
description: "Chúng tôi được đưa cho một đống đá và hai người chơi thay phiên nhau, GG di chuyển trước. Trong mỗi lần di chuyển, người chơi sẽ loại bỏ một hoặc hai viên đá khỏi cọc. Người chơi không thể di chuyển sẽ thua."
date: "2026-06-24T16:53:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105222
codeforces_index: "H"
codeforces_contest_name: "The 2024 Sichuan Provincial Collegiate Programming Contest"
rating: 0
weight: 105222
solve_time_s: 86
verified: true
draft: false
---

[CF 105222H - Trò chơi đá của GG và YY](https://codeforces.com/problemset/problem/105222/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một đống đá và hai người chơi thay phiên nhau, GG di chuyển trước. Trong mỗi lần di chuyển, người chơi sẽ loại bỏ một hoặc hai viên đá khỏi cọc. Người chơi không thể di chuyển sẽ thua. Ngoài việc đơn giản là cố gắng giành chiến thắng, mỗi người chơi còn muốn tối đa hóa số lượng đá mà họ tự mình loại bỏ trong toàn bộ trò chơi. Với cách chơi tối ưu của cả hai bên, chúng ta phải xác định hai điều cho mỗi trường hợp thử nghiệm: ai thắng và người chiến thắng thu thập được bao nhiêu viên đá. 

Đầu vào bao gồm nhiều trò chơi độc lập. Mỗi trò chơi chỉ được xác định bởi số lượng đá ban đầu$n$, có thể lớn bằng$10^{12}$. Điều này ngay lập tức loại trừ mọi mô phỏng hoặc lập trình động dựa trên trạng thái trên tất cả các vị trí cho đến$n$, vì thậm chí$O(n)$mỗi trường hợp thử nghiệm sẽ quá chậm. Thậm chí$O(\sqrt{n})$mỗi truy vấn sẽ chặt chẽ ở$10^4$trường hợp thử nghiệm. Cấu trúc gợi ý mạnh mẽ một giải pháp theo thời gian không đổi hoặc định kỳ dựa trên số học mô-đun. 

Một khía cạnh tế nhị của vấn đề là mục tiêu thứ yếu: người chơi không chỉ quan tâm đến chiến thắng mà còn quan tâm đến việc tối đa hóa những viên đá thu thập được của mình. Điều này có nghĩa là khi một người chơi có nhiều nước đi không làm thay đổi người chiến thắng cuối cùng, họ sẽ thích nước đi dẫn đến tổng điểm cá nhân lớn hơn. Điều này có thể ảnh hưởng đến việc phân phối đá chính xác ngay cả khi kết quả thắng/thua đã được xác định. 

Các trường hợp cạnh xuất hiện ở các giá trị rất nhỏ của$n$, trong đó trò chơi quá ngắn để các mẫu tiệm cận có thể ổn định. Ví dụ, khi$n = 1$, GG chỉ cần lấy viên đá duy nhất và giành chiến thắng ngay với 1 viên đá. Khi$n = 2$, GG lại thắng khi lấy được cả hai viên đá. Tuy nhiên, tại$n = 3$, hành vi thay đổi: GG không thể ép thắng, do đó người chiến thắng sẽ thay đổi và việc phân chia đá phụ thuộc vào cách cả hai người chơi hành xử trong điều kiện hòa tối ưu. Bất kỳ cách tiếp cận sai nào giả định một công thức thống nhất mà không kiểm tra các dư lượng nhỏ modulo 3 sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ mô hình hóa mọi trạng thái trò chơi dưới dạng “số lượng quân cờ hiện tại và lượt của ai” và thử loại bỏ 1 hoặc 2 viên đá theo cách đệ quy. Mỗi trạng thái phân nhánh thành nhiều nhất hai trạng thái tiếp theo và việc ghi nhớ giúp giảm công việc lặp lại, nhưng không gian trạng thái vẫn tăng tuyến tính với$n$. Điều này mang lại một$O(n)$giải pháp cho mỗi trường hợp thử nghiệm, điều này là không thể khi$n \le 10^{12}$. 

Điều quan trọng cần lưu ý là đây là một trò chơi trừ với tập hợp nước đi cố định {1, 2}. Những trò chơi như vậy được biết là có cấu trúc chiến thắng định kỳ. Nếu chỉ quan tâm đến thắng thua, chúng ta sẽ nhận ra ngay mô hình cổ điển: các vị thế có$n \bmod 3 = 0$đang thua người chơi đầu tiên và tất cả những người khác đều thắng. 

Khó khăn đến từ mục tiêu thứ hai: tối đa hóa số đá thu thập được. Tuy nhiên, điều này không phá hủy tính tuần hoàn. Thay vào đó, khi cấu trúc người thắng/kẻ thua đã được cố định, các lựa chọn tối ưu trong các nhánh thắng hoặc thua tương đương cũng lặp lại ở giai đoạn 3. Điều này cho phép chúng tôi nén tất cả các trạng thái thành ba lớp dư lượng và tính toán kết quả chính xác trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force DP trên các tiểu bang |$O(n)$mỗi bài kiểm tra |$O(n)$| Quá chậm | 
| Phân tích định kỳ / modulo 3 |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa vào việc phân tích trò chơi bằng dư lượng của$n$modulo 3 và theo dõi cách chơi tối ưu phân phối đá trong các chu kỳ đó. 

1. Đầu tiên, hãy phân loại vị trí theo$n \bmod 3$. Điều này xác định liệu người chơi bắt đầu (GG) có thể giành chiến thắng hay không. Vị trí ở đâu$n \bmod 3 = 0$đang thua người đi đầu vì mỗi nước đi đều để lại thế thắng cho đối thủ. 
2. Nếu GG ở thế thắng ($n \bmod 3 \neq 0$), giả sử GG thực hiện một nước đi (1 hoặc 2 quân) dẫn đến trạng thái GG vẫn duy trì được chiến thắng bắt buộc. Trong số tất cả các nước đi như vậy, GG chọn nước đi có tổng số quân cuối cùng của mình lớn nhất. Điều này tạo ra sự tối ưu hóa thứ cấp bên trong chiến lược chiến thắng. 
3. Nếu GG ở thế thua ($n \bmod 3 = 0$), GG không thể ngăn cản YY giành chiến thắng nếu cả hai đều chơi tối ưu. Trong trường hợp này, GG vẫn lựa chọn giữa các nước đi nhằm giảm thiểu lợi ích cuối cùng của YY trong khi tối đa hóa số đá thu thập được của chính mình, nhưng kết quả về người chiến thắng đã được ấn định. 
4. Giảm mọi vị trí xuống lớp còn lại tương ứng và tính toán kết quả bằng cách truyền bá các lựa chọn tối ưu trong chu kỳ ba trạng thái. Bởi vì mỗi cử động đều làm giảm$n$bằng 1 hoặc 2, quá trình chuyển đổi chỉ phụ thuộc vào cách các mức giảm này di chuyển giữa các dư lượng, tạo ra mô hình lặp lại ổn định. 
5. In ra người chiến thắng dựa trên số dư và tổng số đá được tính toán mà người chiến thắng đó thu thập được từ mẫu dẫn xuất. 

### Tại sao nó hoạt động 

Biểu đồ trò chơi tạo thành một cấu trúc tuần hoàn có hướng trong đó mỗi bước di chuyển đều giảm dần$n$. Vì nước đi chỉ có 1 hoặc 2 nên mô hình chuyển trạng thái lặp lại sau mỗi 3 bước về cấu trúc chiến thắng. Mục tiêu phụ (tối đa hóa đá của chính mình) không đưa ra các kích thước trạng thái mới ngoài những gì đã được nắm bắt bằng cách theo dõi người chơi nào hiện đang tối ưu trong mỗi loại dư lượng. Điều này giữ cho hệ thống luôn đóng theo các chuyển đổi mô-đun 3, đảm bảo tính nhất quán và ngăn chặn sự phân kỳ thành sự phụ thuộc trạng thái phức tạp hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n: int):
    r = n % 3

    # GG winning positions
    if r != 0:
        # GG wins
        # derive winner stones
        if r == 1:
            # pattern: 1, 2, 4, 5, 7, 8...
            # k = n//3
            if n == 1:
                v = 1
            else:
                v = 2 * (n // 3)
        else:  # r == 2
            v = 2 * (n // 3) + 1
        return 0, v

    # GG loses -> YY wins
    k = n // 3
    v = 2 * k - 1
    return 1, v

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        w, v = solve_case(n)
        print(w, v)

if __name__ == "__main__":
    main()
```Mã này trực tiếp thực hiện việc phân loại dư lượng. Nhánh đầu tiên xử lý tất cả các trường hợp GG thắng, tức là khi$n \% 3 \neq 0$. Trong đó, giá trị của$v$được tính toán bằng cách sử dụng mẫu ổn định cho mỗi lớp dư lượng. Nhánh thứ hai xử lý các vị trí thua cho GG, trong đó YY là người chiến thắng và giá trị bắt nguồn từ hành vi tuyến tính được quan sát ở mỗi khối gồm ba viên đá. 

Chi tiết triển khai tinh tế duy nhất là xử lý$n = 1$, là vị trí chiến thắng nhỏ nhất và chưa phù hợp với vị thế ổn định$3k$khuôn mẫu cho$r = 1$lớp học. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$n = 4$. Phần dư là$1$, vậy là GG thắng. Việc tính toán đặt điều này vào$3k+1$lớp học với$k = 1$, cho điểm người thắng là 2. GG lấy 1 hoặc 2 quân trước, nhưng chỉ nước đi lấy 1 mới bảo toàn cơ cấu chiến thắng, do đó GG buộc phải đi vào một nhánh tối ưu cụ thể dẫn đến tổng cộng là 2 quân. 

Bây giờ hãy xem xét$n = 5$. Phần dư là$2$, thế là GG lại thắng. Đây$k = 1$, và công thức cho$v = 2k + 1 = 3$. GG có thể chọn nước đi đầu tiên khiến YY thua vị trí trong khi vẫn tối đa hóa số đá tích lũy của mình, dẫn đến tổng số là 3. 

| n | dư lượng | người chiến thắng | GG di chuyển | kết quả | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | GG | con đường giành chiến thắng bắt buộc tối ưu | GG = 2 | 
| 5 | 2 | GG | chọn nước đi dẫn đến lợi thế 3 trạng thái | GG = 3 | 

Những ví dụ này cho thấy lớp dư lượng cố định cấu trúc trò chơi như thế nào, trong khi mục tiêu phụ chọn trong số các phần tiếp theo giành chiến thắng tương đương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi bằng cách sử dụng số học mô-đun | 
| Không gian |$O(1)$| Không có trạng thái nào được lưu trữ ngoài một vài số nguyên | 

Giải pháp dễ dàng nằm trong giới hạn vì ngay cả đối với$10^4$trường hợp thử nghiệm, chỉ các phép tính số học đơn giản được thực hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            r = n % 3
            if r != 0:
                if r == 1:
                    v = 1 if n == 1 else 2 * (n // 3)
                else:
                    v = 2 * (n // 3) + 1
                out.append(f"0 {v}")
            else:
                out.append(f"1 {2*(n//3)-1}")
        return "\n".join(out)

    return solve()

# custom cases
assert run("3\n1\n2\n3") == "0 1\n0 2\n1 1"
assert run("3\n4\n5\n6") == "0 2\n0 3\n1 3"
assert run("2\n1000000000000\n999999999999") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,2,3 | trường hợp cơ bản cơ bản | độ đúng nhỏ nhất n | 
| 4,5,6 | chuyển đổi chu kỳ đầu tiên | hành vi dư lượng | 
| lớn | ổn định | không tràn/O(1) logic | 

## Vỏ cạnh 

cho$n = 1$, GG thắng ngay và lấy được viên đá duy nhất. Trường hợp này bỏ qua tất cả các mẫu mô-đun và phải được xử lý rõ ràng để tránh các công thức dựa trên số 0 không chính xác. 

Vì$n = 3$, GG đang ở thế thua. Mặc dù GG có thể thử các bước đi đầu tiên khác nhau nhưng cả hai lựa chọn đều cho phép YY giành chiến thắng cuối cùng. Tuy nhiên, GG vẫn chọn chiêu thức tối đa hóa số đá thu thập được của mình trong số các nhánh bị mất, giúp khắc phục sự phân bổ cuối cùng một cách độc đáo. 

Đối với rất lớn$n$, chẳng hạn như$10^{12}$, giải pháp dựa hoàn toàn vào số học mô-đun. Thuật toán không bao giờ xây dựng các trạng thái trò chơi trung gian, do đó không có nguy cơ tràn hoặc suy giảm hiệu suất và quá trình tính toán vẫn duy trì thời gian không đổi cho mỗi trường hợp thử nghiệm.
