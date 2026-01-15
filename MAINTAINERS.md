# Maintainers Guide

This document provides guidance for maintaining the nv-i18n library, updating ISO code data, and creating releases to Maven Central.

## Project Overview

The nv-i18n library is a Java package providing support for internationalization with enums for various ISO standards:

- **CountryCode**: ISO 3166-1 country codes
- **LanguageCode**: ISO 639-1 language codes  
- **LanguageAlpha3Code**: ISO 639-2 language codes
- **LocaleCode**: Available locales in 'xx' or 'xx-XX' format
- **ScriptCode**: ISO 15924 script codes
- **CurrencyCode**: ISO 4217 currency codes

## Data Maintenance

### Updating ISO Code Data

The library's enum classes contain data based on various ISO standards. When these standards are updated, the corresponding Java enum classes need to be updated.

#### Sources for Updates

- **Country Codes (CountryCode.java)**: [ISO 3166-1](https://www.iso.org/iso-3166-country-codes.html)
- **Language Codes (LanguageCode.java)**: [ISO 639-1](https://www.iso.org/iso-639-language-codes.html)  
- **3-Letter Language Codes (LanguageAlpha3Code.java)**: [ISO 639-2](https://www.loc.gov/standards/iso639-2/php/code_list.php); changes are published in a newsletter available [here](https://www.six-group.com/en/products-services/financial-information/market-reference-data/data-standards.html)
- **Script Codes (ScriptCode.java)**: [ISO 15924](https://www.unicode.org/iso15924/iso15924-codes.html)
- **Currency Codes (CurrencyCode.java)**: [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html)
- **Locale Codes (LocaleCode.java)**: Based on Java's available locales

#### Update Process

1. **Check for Updates**: Regularly monitor the ISO standards for new codes, name changes, or deprecations
2. **Update Enum Classes**: Modify the appropriate Java enum class in `src/main/java/com/nv_i18n/i18n/`
3. **Handle Deprecations**: Mark deprecated codes using `@Deprecated` annotation and add documentation
4. **Update Javadocs**: Ensure all new codes have proper documentation
5. **Run Tests**: Execute the test suite to ensure all changes are valid
6. **Update CHANGES.md**: Document all changes in the changelog

#### Example Update Process

```bash
# Run tests to ensure changes don't break existing functionality
mvn test

# Compile and package to verify everything builds correctly
mvn clean package
```

### Code Maintenance Guidelines

- **Maintain Alphabetical Order**: Keep enum constants in alphabetical order by code
- **Consistent Naming**: Use uppercase for enum constants, matching the ISO standard codes
- **Documentation**: Each enum constant should have proper Javadoc documentation
- **Deprecation**: When marking codes as deprecated, include information about replacements
- **Thread Safety**: All enum classes should remain thread-safe and immutable

## Release Process

### Prerequisites

Before creating a release, ensure you have:

1. **GPG Key**: Set up for signing artifacts
2. **Sonatype Account**: Access to the `io.github.nv-i18n` group
3. **Maven Settings**: Proper server configuration in `~/.m2/settings.xml`

### Maven Settings Configuration

Add the following to your `~/.m2/settings.xml`:

```xml
<settings>
  <servers>
    <server>
      <id>central</id>
      <username>YOUR_SONATYPE_USERNAME</username>
      <password>YOUR_SONATYPE_PASSWORD</password>
    </server>
  </servers>
  
  <profiles>
    <profile>
      <id>gpg</id>
      <properties>
        <gpg.executable>gpg</gpg.executable>
        <gpg.keyname>YOUR_GPG_KEY_ID</gpg.keyname>
      </properties>
    </profile>
  </profiles>
  
  <activeProfiles>
    <activeProfile>gpg</activeProfile>
  </activeProfiles>
</settings>
```

### Release Steps

1. **Prepare the Release**
   ```bash
   # Ensure your working directory is clean
   git status
   
   # Update version in pom.xml
   # Update CHANGES.md with release date and changes
   ```

2. **Create Release Artifacts**
   ```bash
   # Clean and build with all required artifacts
   mvn clean compile test package
   ```

3. **Deploy to Maven Central**
   ```bash
   # Deploy using the Central Publishing Maven Plugin
   mvn deploy -DskipTests -Dmaven.deploy.skip=true
   ```

### Troubleshooting Release Issues

- **GPG Signing Issues**: Ensure GPG agent is running and key is available
- **Central Publishing Issues**: Check Sonatype OSSRH status and credentials
- **Build Failures**: Review test results and compilation errors
- **Version Conflicts**: Ensure version numbers follow semantic versioning

### Release Checklist

- [ ] All tests pass (`mvn test`)
- [ ] Documentation is up-to-date
- [ ] CHANGES.md includes all changes for the release
- [ ] Version number follows semantic versioning
- [ ] GPG signing is configured and working
- [ ] Sonatype credentials are valid
- [ ] Release artifacts are generated successfully
- [ ] Release is deployed to Maven Central

## Versioning Strategy

This project follows [Semantic Versioning](https://semver.org/):

- **MAJOR**: Incompatible API changes
- **MINOR**: New functionality in backward-compatible manner
- **PATCH**: Backward-compatible bug fixes

### When to Increment Versions

- **Major Version**: Breaking changes, major API restructuring
- **Minor Version**: New ISO codes added, new enum constants
- **Patch Version**: Bug fixes, documentation updates, deprecated code removals

## Contributing Guidelines

1. **Fork and Clone**: Fork the repository and clone your fork
2. **Create Feature Branch**: Create a branch for your changes
3. **Make Changes**: Update the appropriate enum classes
4. **Add Tests**: Ensure test coverage for new functionality
5. **Update Documentation**: Update CHANGES.md and relevant documentation
6. **Submit Pull Request**: Create a PR with clear description of changes

## Contact Information

For questions about maintenance or releases:

- **GitHub Issues**: [nv-i18n Issues](https://github.com/nv-i18n/nv-i18n/issues)
- **Project Repository**: [nv-i18n](https://github.com/nv-i18n/nv-i18n)

## Resources

- [Maven Central Publishing Guide](https://central.sonatype.org/publish/publish-guide/)
- [GPG Setup for Maven](https://central.sonatype.org/publish/requirements/gpg/)
- [Semantic Versioning](https://semver.org/)
- [Contributing Guidelines](CODE_OF_CONDUCT.md)
