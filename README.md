# Clean Architecture in Flutter

Clean Architecture is a software design philosophy that separates the elements of a design into ring levels. This separation helps to achieve a high degree of separation of concerns and maintainability. In this document, we will explore how to implement Clean Architecture in a Flutter application using the following packages: `Freezed`, `dartz`, `chopper`, and `riverpod`.

## Table of Contents

1. [Overview of Clean Architecture](#overview-of-clean-architecture)
2. [Setting up the Project](#setting-up-the-project)
3. [Project Structure](#project-structure)
4. [Using Freezed for Immutable Data Classes](#using-freezed-for-immutable-data-classes)
5. [Functional Programming with Dartz](#functional-programming-with-dartz)
6. [Networking with Chopper](#networking-with-chopper)
7. [State Management with Riverpod](#state-management-with-riverpod)
8. [Integrating Layers and Example Usage](#integrating-layers-and-example-usage)
9. [References](#references)

## Overview of Clean Architecture

Clean Architecture divides the application into layers with specific responsibilities. The most common layers are:

![Uncle bob Clean Architecture](https://github.com/monocsp/flutter_clean_architecture/blob/main/CleanArchitecture.jpg?raw=true)

- **Presentation Layer**: Contains the UI and manages user interactions.
- **Domain Layer**: Contains business logic and domain entities.
- **Data Layer**: Handles data operations such as network requests and database interactions.

Each layer communicates with the other via interfaces or abstractions, ensuring that changes in one layer do not affect others.

## Setting up the Project

First, create a new Flutter project:

```bash
flutter create flutter_clean_architecture
cd flutter_clean_architecture
```

Add the necessary dependencies to `pubspec.yaml`:

```bash
dart pub add dev:build_runner
dart pub add dev:freezed

dart pub add freezed
dart pub add dartz

flutter pub add chopper
flutter pub add riverpod
```

```yaml
dependencies:
  flutter:
    sdk: flutter
  freezed_annotation: newest_version
  dartz: newest_version
  chopper: newest_version
  riverpod: newest_version

dev_dependencies:
  build_runner: newest_version
  freezed: newest_version
```

Run `flutter pub get` to install the packages.

## Project Structure

Here is an example of a Clean Architecture project structure:

```
lib/
├── core/
│   ├── abstract/
│   ├── api/
│   ├── controller/
│   ├── errors/
│   ├── extensions/
│   ├── enums/
│   ├── permissions/
│   ├── routes/
│   ├── themes/
│   ├── usecases/
│   └── utils/
├── features/
│   └── feature_name/
│       ├── data/
│       │   ├── datasources/
│       │   ├── models/
│       │   └── repositories/
│       ├── domain/
│       │   ├── entities/
│       │   ├── repositories/
│       │   └── usecases/
│       └── presentation/
│           ├── pages/
│           └── providers/
└── main.dart
```

## Using Freezed for Immutable Data Classes

Freezed helps to create immutable classes with union types and provides JSON serialization. Create a model in `lib/features/feature_name/data/models/`.

```dart
import 'package:freezed_annotation/freezed_annotation.dart';

part 'example_model.freezed.dart';
part 'example_model.g.dart';

@freezed
class ExampleModel with _$ExampleModel {
  const factory ExampleModel({
    required String id,
    required String name,
  }) = _ExampleModel;

  factory ExampleModel.fromJson(Map<String, dynamic> json) => _$ExampleModelFromJson(json);
}
```

Generate the code using the following command:

```bash
flutter pub run build_runner build
```

## Functional Programming with Dartz

Dartz provides functional programming tools like `Either` for handling errors and `Option` for nullable values. Use `Either` in your use cases:

```dart
import 'package:dartz/dartz.dart';
import 'package:clean_architecture_flutter/core/errors/failures.dart';
import 'package:clean_architecture_flutter/features/feature_name/domain/entities/example_entity.dart';
import 'package:clean_architecture_flutter/features/feature_name/domain/repositories/example_repository.dart';

class GetExample {
  final ExampleRepository repository;

  GetExample(this.repository);

  Future<Either<Failure, ExampleEntity>> excute(String id) async {
    return await repository.getExample(id);
  }
}
```

## Networking with Chopper

Chopper is used for HTTP requests. Define your API service in `lib/core/network/`:

```dart
import 'package:chopper/chopper.dart';

part 'example_service.chopper.dart';

@ChopperApi()
abstract class ExampleService extends ChopperService {
  @Get(path: '/example/{id}')
  Future<Response> getExample(@Path('id') String id);

  static ExampleService create() {
    final client = ChopperClient(
      baseUrl: 'https://api.example.com',
      services: [_$ExampleService()],
      converter: JsonConverter(),
      interceptors: [
          HttpLoggingInterceptor(),
          ResponseErrorInterceptor()
        ],
    );
    return _$ExampleService(client);
  }
}
```

Generate the code using the following command:

```bash
flutter pub run build_runner build
```

## State Management with Riverpod

Riverpod provides a more flexible and testable state management solution. Define your providers in `lib/features/feature_name/presentation/providers/`.

```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:dartz/dartz.dart';
import 'package:clean_architecture_flutter/core/errors/failures.dart';
import 'package:clean_architecture_flutter/features/feature_name/domain/entities/example_entity.dart';
import 'package:clean_architecture_flutter/features/feature_name/domain/usecases/get_example.dart';
import 'package:clean_architecture_flutter/features/feature_name/data/repositories/example_repository_impl.dart';
import 'package:clean_architecture_flutter/features/feature_name/data/datasources/example_remote_datasource.dart';
import 'package:clean_architecture_flutter/features/feature_name/data/services/example_service.dart';

@Riverpod(keepAlive: true)
@riverpod
class ExampleProvider extends _$ExampleProvider {
  @override
  AsyncValue<ExampleEntity> build() => AsyncData(ExampleEntity.empty());

  Future<void> fetchExample({required String id}) async {
    if (state.isLoading) return;
    state = const AsyncLoading();
    try {
      ExampleRepository exampleRepository = ExampleRepositoryImpl(
          remoteDatasource: ExampleRemoteDatasourceImpl(
              exampleService: ExampleService.create()));

      Either<Failure, ExampleEntity> result =
          await GetExample(repository: exampleRepository).call(id);

      if (result.isRight()) {
        state = AsyncData(result.getOrElse(() => ExampleEntity.empty()));
        return;
      }

      state = AsyncError(result.fold((l) => l, (r) => null), StackTrace.current);
    } catch (error) {
      state = AsyncError(
          Failure(message: 'Example Provider error $error'), StackTrace.current);
    }
  }

  void reset() {
    state = AsyncData(ExampleEntity.empty());
  }
}

```

## Integrating Layers and Example Usage

In your UI layer, you can use the providers to manage state and handle user interactions. For example, in `lib/features/feature_name/presentation/pages/`:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:clean_architecture_flutter/features/feature_name/presentation/providers/example_provider.dart';

class ExamplePage extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final exampleState = ref.watch(exampleProvider);

    return Scaffold(
      appBar: AppBar(title: Text('Example')),
      body: exampleState.when(
        data: (example) => Text(example.name),
        loading: () => CircularProgressIndicator(),
        error: (error, stack) => Text('Error: $error'),
      ),
    );
  }
}
```

## Conclusion

This document provides a basic overview of implementing Clean Architecture in a Flutter application using `Freezed`, `dartz`, `chopper`, and `riverpod`. By following these principles and leveraging these packages, you can create a maintainable, scalable, and testable Flutter application.

## References

- [Freezed Documentation](https://pub.dev/packages/freezed)
- [Freezed_annotation Documentation](https://pub.dev/packages/freezed_annotation)
- [Dartz Documentation](https://pub.dev/packages/dartz)
- [Chopper Documentation](https://pub.dev/packages/chopper)
- [Riverpod Documentation](https://pub.dev/packages/riverpod)
- [Build_Runner Documentation](https://pub.dev/packages/build_runner)
- [AsyncValue Documentation](https://pub.dev/documentation/riverpod/latest/riverpod/AsyncValue-class.html)
