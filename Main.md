import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Main {

    public static void main(String[] args) {
        List<String> names = Arrays.asList("Jack", "Connor", "Harry", "George", "Samuel", "John");
        List<String> families = Arrays.asList("Evans", "Young", "Harris", "Wilson", "Davies", "Adamson", "Brown");
        Collection<Person> persons = new ArrayList<>();
        for (int i = 0; i < 10_000_000; i++) {
            persons.add(new Person(
                    names.get(new Random().nextInt(names.size())),
                    families.get(new Random().nextInt(families.size())),
                    new Random().nextInt(100),
                    Sex.values()[new Random().nextInt(Sex.values().length)],
                    Education.values()[new Random().nextInt(Education.values().length)])
            );
        }

        Stream<Person> stream1 = persons.stream()
                .filter(x -> x.getAge() < 18);
        System.out.println(stream1.count());

        Stream<String> stream2 = persons.stream()
                .filter(x -> x.getSex().equals(Sex.MAN))
                .filter(x -> x.getAge() >= 18 && x.getAge() <= 27)
                .map(Person::getFamily);
        List<String> army = new ArrayList<>();
        army.add(String.valueOf(stream2.collect(Collectors.toList())));

        Stream<String> stream3 = persons.stream()
                .filter(x -> x.getEducation().equals(Education.HIGHER))
                .filter(x -> x.getSex().equals(Sex.MAN) && x.getAge() > 18 && x.getAge() < 65)
                .filter(x -> x.getSex().equals(Sex.WOMAN) && x.getAge() > 18 && x.getAge() < 60)
                .sorted(Comparator.comparing(Person::getFamily))
                .map(Person::getFamily);
        List<String> pay = new ArrayList<>();
        pay.add(String.valueOf(stream3.collect(Collectors.toList())));



    }


    public enum Sex {
        MAN,
        WOMAN
    }

    public enum Education {
        ELEMENTARY,
        SECONDARY,
        FURTHER,
        HIGHER
    }

}